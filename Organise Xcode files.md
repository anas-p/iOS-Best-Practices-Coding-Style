# Way to Organize Project Files in Xcode
- **Data Manager/**
    - Data/
        - DataStack1
        - DataStack2
        - etc
    - Network/
        - NetworkRouter
        - etc
    - Util/
        - foundation extensions, helper classes
        
- **Library/**
    - External library 1
    - External library 2
    
- **Storyboards/**
    - Tab1.storyboard
    - Tab2.storyboard
    
- **UI/**
    - Tab1 (or Functionality Area 1)/
        - ViewController1
        - ViewController2
        - Views/
    - Tab2 (or Functionality Area 2)/
        - ViewController1
        - ViewController2
        - Views/
    - Common/
        - SharedUIComponent1
        - SharedUIComponent2
        
- **Utilities/**
    - Extensions of Cocoa classes
    - Colors helper
    - Fonts helper
    - etc
    
- **Resources/**
    - Fonts/
    - Placeholder images (final images go into .XCAssets file)
    - etc


### Data Manager
- separate directories for each kind of data being managed (usually a DataManager singleton and a collection of object classes).
- A directory for all the networking code.
- A directory for any extensions or other backend/data helper classes.
- API Request classes
- Core data classes

### Storyboards
- Simple flat directory for the collection of storyboards. Makes them easy to find.

### Library
- Each external library gets a folder here. A lot of the time it's easier to include this way (especially if it requires changes) than doing a link with CocoaPods or Carthage.

### UI
- Each area of the app gets its own folder. This might be by tab, or by other high level UI "concept." Within the folder there are the appropriate view controllers, and a sub folder for all the custom views. If there are a LOT of views the Views folder might be further broken down

- There's also a Common folder. Any UI component that gets used across more than one UI folder gets promoted to Common. Common gets further organized when it gets big.

- The UI/Util folder is for things like a Color manager, Fonts manager, and extensions to the Cocoa classes.

### Resources
- This is where fonts live. It might also hold things like string tables for localization, and placeholder images during development (final images are all in XCAssets, but that file can get burly, so it's nice to have placeholder stuff here so you know to delete it)

