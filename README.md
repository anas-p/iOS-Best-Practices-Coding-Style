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
//Protocol name should be end purpose of protocol
//ex: FooDelegate , FooDataSource
protocol ProtocolNameDelegate {
    func foo(param1: String, param2: Bool)
}

class MyViewcontroller : UIViewController{

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
    
    // MARK: Additional Helpers
    private func displayNameForFoo(foo: Foo) {
        // ...
    }
}

// MARK: - UITableViewDataSource
extension MyViewcontroller: UITableViewDataSource {
  // Table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewcontroller: UIScrollViewDelegate {
  // Scroll view delegate methods
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

## Must Follow:
- **Native Swift Types** - Use Swift types whenever possible (Array, Dictionary, Set, String, etc.) as opposed to the NS* types from Objective-C

    **Incorrect**
    ```swift
    let pageLabelText = NSString(format: "%@/%@", currentPage, pageCount)
    
    //Do not make NSArray, NSDictionary, and NSSet properties or variables
    var arrayOfJSONObjects: NSArray = NSArray()
    ...
    let names: AnyObject? = arrayOfJSONObjects.value(forKeyPath: "name")
    ```
    **Correct**
    ```swift
    let pageLabelText = "\(currentPage)/\(pageCount)"
    let alsoPageLabelText = currentPage + "/" + pageCount
    
    //cast Swift type in order to use objectice-C method.
    var arrayOfJSONObjects = [[String: AnyObject]]()
    ...
    let names: AnyObject? = (arrayOfJSONObjects as NSArray).value(forKeyPath: "name")
    ```
    
- **Avoid force unwrapping optionals** by using `!` or `as!` as this will cause app to crash if the value trying to use is `nil`. Safely unwrap the optional first by using things like `guard let`, `if let`, `guard let as?`, `if let as?`, and optional chaining.

    ### Error Handling:
    - **Forced-try Expression**
        - **Avoid using the forced-try expression**: `try!`
        
            **Incorrect**
            ```swift
            // This will crash at runtime if there is an error parsing the JSON data!
            let json = try! JSONSerialization.jsonObject(with: data, options: .allowFragments)
            print(json)
            ```
            **Correct**
            ```swift
            do {
                let json = try JSONSerialization.jsonObject(with: data, options: .allowFragments)
                print(json)
            } catch {
                print(error)
            }
            ```
    - **Let vs. Var**
        - Whenever possible use let instead of var.
        - Declare properties of an **object** or **struct** that shouldn't change over its lifetime with `let`.
        
    - **Access Control**
        - Prefer `private` properties and methods whenever possible to encapsulate and limit access to internal object state.
        - For `private` declarations at the top level of a file that are outside of a type, explicitly specify the declaration as `fileprivate`. This is functionally the same as marking these declarations private, but clarifies the scope:
        
            **Incorrect**
            ```swift
            import Foundation

            // Top level declaration
            private let foo = "bar"

            struct Baz {
            ...
            ```
            **Correct**
            ```swift
            import Foundation

            // Top level declaration
            fileprivate let foo = "bar"

            struct Baz {
            ...
            ```
        - If you need to expose functionality to other modules, prefer `public` classes and class members whenever possible to ensure functionality is not accidentally overridden. Better to expose the class to `open` for subclassing when needed.
        
    ### Spacing
    - Open curly braces on the same line as the statement and close on a new line.
    - Put `else` statements on the same line as the closing brace of the previous `if` block.
    - Make all colons left-hugging (no space before but a space after) except when used with the ternary operator (a space both before and after).
        
         **Incorrect**
        ```swift
        class SomeClass : SomeSuperClass
        {
            private let someString:String

            func someFunction(someParam :Int)
            {
                let dictionaryLiteral : [String : AnyObject] = ["foo" : "bar"]

                let ternary = (someParam > 10) ? "foo": "bar"

                if someParam > 10 { ... }

                else {
                        ...
                } } }
        ```
        **Correct**
        ```swift
        class SomeClass: SomeSuperClass {
            private let someString: String
            func someFunction(someParam: Int) {
                let dictionaryLiteral: [String: AnyObject] = ["foo": "bar"]

                let ternary = (someParam > 10) ? "foo" : "bar"

                if someParam > 10 {
                    ...
                } else {
                    ...
                }
            }
        }
        ```
    ### Protocols
    - **Protocol Conformance**
        - When adding protocol conformance to a type, use a separate extension for the protocol methods. This keeps the related methods grouped together with the protocol and can simplify instructions to add a protocol to a type with its associated methods.
        - Use a `// MARK: - SomeDelegate` comment to keep things well organized.
        
            **Incorrect**
            ```swift
            class MyViewcontroller: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
                // All methods
            }
            ```
            **Correct**
            ```swift
            class MyViewcontroller: UIViewController {
                ...
            }

            // MARK: - UITableViewDataSource
            extension MyViewcontroller: UITableViewDataSource {
                // Table view data source methods
            }

            // MARK: - UIScrollViewDelegate
            extension MyViewcontroller: UIScrollViewDelegate {
                // Scroll view delegate methods
            }
            ```
            
    - **Delegate Protocols**
        - If your protocol should have **optional methods**, it must be declared with the `@objc` attribute.
        - Declare protocol definitions near the class that uses the delegate, not the class that implements the delegate methods.
        - If more than one class uses the same protocol, declare it in its own file.
        - Use `weak` optional `var`s for delegate variables to avoid retain cycles.
    
        ```swift
        //SomeTableCell.swift
        protocol SomeTableCellDelegate: class {
            func cellButtonWasTapped(cell: SomeTableCell)
        }

        class SomeTableCell: UITableViewCell {
            weak var delegate: SomeTableCellDelegate?
            // ...
        }
        ```
        ```swift
        //SomeTableViewController.swift
        class SomeTableViewController: UITableViewController {
            // ...
        }

        // MARK: - SomeTableCellDelegate
        extension SomeTableViewController: SomeTableCellDelegate {
            func cellButtonWasTapped(cell: SomeTableCell) {
                // Implementation of cellbuttonwasTapped method
            }
        }
        ```
    ### Arrays and Dictionaries
    - **Type Shorthand Syntax**
    
        Use square bracket shorthand type syntax for Array and Dictionary as recommended by Apple in [Array Type Shorthand Syntax](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/CollectionTypes.html#//apple_ref/doc/uid/TP40014097-CH8-ID107):
        
        **Incorrect**
        ```swift
        let users: Array<String>
        let usersByName: Dictionary<String, User>
        ```
        **Correct**
        ```swift
        let users: [String]
        let usersByName: [String: User]
        ```
    - **Trailing Comma**
    
        For array and dictionary literals, unless the literal is very short, split it into multiple lines, with the opening symbols on their own line, each item or key-value pair on its own line, and the closing symbol on its own line. Put a trailing comma after the last item or key-value pair to facilitate future insertion/editing. Xcode will handle alignment sanely.
    
        **Incorrect**
        ```swift
        let anArray = [
            object1,
            object2,
            object3 //no trailing comma
        ]

        let aDictionary = ["key1": value1, "key2": value2] //how can you even read that?!
        ```
        **Correct**
        ```swift
        let anArray = [
            object1,
            object2,
            object3,
        ]

        let aDictionary = [
            "key1": value1,
            "key2": value2,
        ]
        ```
        
## Resources:

- [Swift.org -  API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)
- [nshipster.com - Swift Documentation](http://nshipster.com/swift-documentation/)
- [Futurice - ios-good-practices](https://github.com/anasamanp/ios-good-practices)
- [Vokal Engineering - Swift Coding Standards](https://engineering.vokal.io/iOS/CodingStandards/Swift.md.html#declaring-variables)
- [RW - Swift Style Guide](https://github.com/anasamanp/swift-style-guide)


    
    
