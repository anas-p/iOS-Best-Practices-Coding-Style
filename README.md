# iOS Best Practices for Coding Style
## Fundamentals:
- **Clarity at the point of use** is most important goal. Entities such as methods and properties are declared only once but used repeatedly.
- **Write a documentation comment** for every declaration. Insights gained by writing documentation can have a profound impact on your design, so don’t put it off.
    - Use Swift’s [dialect of Markdown](https://developer.apple.com/library/content/documentation/Xcode/Reference/xcode_markup_formatting_ref/) for documentation.
    - [Reference for inline documentation](http://nshipster.com/swift-documentation/).
    - **Begin with a summary** that describes the entity being declared. Often, an API can be completely understood from its declaration and its summary.
    
         ```swift
        /// Returns a 'view' of 'self' containing the same elements in
        /// reverse order.
        func reversed() -> ReverseCollection
        ```
        - **Focus on the summary**; it’s the most important part. Many excellent documentation comments consist of nothing more than a great summary.
        - **Use a single sentence fragment** if possible, ending with a period. Do not use a complete sentence.
        - **Describe what a function or method does and what it returns**, omitting null effects and Void returns:
        
## Naming:
**Promote Clear Usage**
- **Include all the words needed to avoid ambiguity** for a person reading code where the name is used.
- **Name variables, parameters, and associated types according to their roles**, rather than their type constraints.
- **Make the name descriptive**.

    **Incorrect**
    ```swift
    var string = "Hello" 
    var textField1 = UITextField()
    @IBOutlet weak var label: UILabel!
    @IBOutlet weak var image: UIImageView!
    ```
   **Correct**
    ```swift
    var greeting = "Hello"
    var txtName = UITextField()
    @IBOutlet weak var lblAge: UILabel!
    @IBOutlet weak var imgProfile: UIImageView!
    ```
## Conventions:
- **Follow case conventions**. Names of types and protocols are `UpperCamelCase`. Everything else is `lowerCamelCase`.

    ```swift
    var utf8Bytes: [UTF8.CodeUnit]
    var isRepresentableAsASCII = true
    var userSMTPServer: SecureSMTPServer
    ```
    ```swift
    var radarDetector: RadarScanner
    var enjoysScubaDiving = true
    ```
## Parameters:
> func move(from **start**: Point, to **end**: Point)

- **Write documention for each Parameter, Throws, and Returns**.

    ```swift
    /**
    Repeats a string `times` times.

    - Parameter str:   The string to repeat.
    - Parameter times: The number of times to repeat `str`.

    - Throws: `MyError.InvalidTimes` if the `times` parameter 
        is less than zero.

    - Returns: A new string with `str` repeated `times` times.
    */
    func repeatString(str: String, times: Int) throws -> String {
        guard times >= 0 else { throw MyError.InvalidTimes }
        return Repeat(count: 5, repeatedValue: "Hello").joinWithSeparator("")
    }
    ```
    
## Structure of a Class file:
Using `MARK:` comment is a great way to group your methods, especially in view controllers.

```swift
/**
Documentation for the class/file
*/

import SomeExternalFramework

//MARK: Protocols declarations
//Protocol name should be ends with protocol purpose
//ex: FooDelegate , FooDataSource
protocol ProtocolNameDelegate {
    func foo(param1: String, param2: Bool)
}

class FooViewController : UIViewController, UITableViewDeleagte, UITableViewDatasource {

    //MARK: Delegate initialization
    var delegate: ProtocolNameDelegate?
    
    //MARK: Outlets
    @IBOutlet weak var tableView: UITableView!
    
    // Custom initializers go here
    private let fooStringConstant = "FooConstant"
    private let floatConstant = 1234.5

    // MARK: View Lifecycle
    override func viewDidLoad() {
        super.viewDidLoad()
        // ...
    }
    
     override func viewDidLayoutSubviews() {
        super.viewDidLayoutSubviews()
        // ...
    }

    // MARK: User Interaction - Actions & Targets
    func foobarButtonTapped() {
        // ...
    }

    // MARK: UITableView Datasource
    func foobar(foobar: Foobar, SomethingWithFoo foo: Foo) {
        // ...
    }

    // MARK: UITableView Delegate
    func foobar(foobar: Foobar, didSomethingWithFoo foo: Foo) {
        // ...
    }
    
    // MARK: Additional Helpers
    private func displayNameForFoo(foo: Foo) {
        // ...
    }
}
//MARK: Extension - Name of extension class
/**
 - Documentation for purpose of extension
*/
extension SomeOtherClass: UIViewController {
    func foobar(foobar: Foobar, somethingWithFoo foo: Foo) {
        // ...
    }
}
```


## Resources:

- [Swift.org -  API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)
- [nshipster.com - Swift Documentation](http://nshipster.com/swift-documentation/)
- [Futurice - ios-good-practices](https://github.com/futurice/ios-good-practices#coding-style)
- [Vokal Engineering - Swift Coding Standards](https://engineering.vokal.io/iOS/CodingStandards/Swift.md.html#declaring-variables)


    
    
