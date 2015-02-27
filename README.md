# AutocompleteTextfieldSwift
Simple and straightforward sublass of UITextfield to manage string suggestions

![Sample Usage](http://i.imgur.com/EJfFLty.png)

## Installation
Drag AutocompleteTextfield Folder in yuor project

##How to use

####Customize
Customize autocomplete suggestions! You can override the provided properties to create your desired appearance.
Properties are pretty self explanatory, but description has been provided as well.
```
 /// Font for the text suggestions
  var autoCompleteTextFont = UIFont(name: "HelveticaNeue-Light", size: 12)
  
  /// Color of the text suggestions
  var autoCompleteTextColor = UIColor.blackColor()
  
  /// Used to set the height of cell for each suggestions
  var autoCompleteCellHeight:CGFloat = 44.0
  
  /// The maximum visible suggestion
  var maximumAutoCompleteCount = 3
  
  /// Used to set your own preferred separator inset
  var autoCompleteSeparatorInset = UIEdgeInsetsZero
  
  /// Hides autocomplete tableview when the textfield is empty
  var hideWhenEmpty = true
  
  /// Hides autocomplete tableview after selecting a suggestion
  var hideWhenSelected = true
```

The most important property to use is the `autoCompleteStrings`. As what is declared in the description setting the value of this will automatically reload the tableview, through the use of didSet
 /// The strings to be shown on as suggestions, setting the value of this automatically reload the tableview
  var autoCompleteStrings:[String]?{
    didSet{
      reloadAutoCompleteData()
    }
  }


####Protocol
Conform with AutocompleteTextFieldDelegate

```
class ViewController: UIViewController, AutocompleteTextFieldDelegate{...}
```

and add:

```
autocompleTextfield.autoCompleteDelegate = self
```

After this you can do whatever you want with the provided delegates! In here i used `textFieldDidChange(text:String)` to request autocomplete places to google api 

```
func textFieldDidChange(text: String) {
    if !text.isEmpty{
      if connection != nil{
        connection!.cancel()
        connection = nil
      }
      let baseURLString = "https://maps.googleapis.com/maps/api/place/autocomplete/json"
      let url = NSURL(string: "\(baseURLString)?key=\(googleMapsKey)&input=\(text)")
      if url != nil{
        let urlRequest = NSURLRequest(URL: url!)
        connection = NSURLConnection(request: urlRequest, delegate: self)
      }
    }
  }
```

and `didSelectAutocompleteText(text:String, indexPath:NSIndexPath)` to process the selected string, in which I performed geocoding using `CLGeocoder` to retrieve placemark and add annotation to our mapview.

It's that easy!

##Example Code:
In the example project, I used [Google Places Autocomplete API](https://developers.google.com/places/documentation/autocomplete) to show the usage of this library. For testing purposes i created my own googl api key

#####Note: _If you want to create your own Google Api Key follow the steps in this [link](https://developers.google.com/maps/documentation/javascript/tutorial#api_key)_

##License
AutocompleteTextfield is under MIT license. See LICENSE for details.

##In the future
- support for attributedText