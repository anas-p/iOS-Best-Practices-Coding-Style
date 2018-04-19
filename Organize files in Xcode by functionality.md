# Organized files in Xcode by functionality
Sample Screenshot:

![Project Files Sample](https://github.com/anasamanp/iOS-Best-Practices-Coding-Style/blob/master/ProjectFolder.png)

- **Constants.swift** or **Enums.swift**, which contains `enum`s for all static constants used in the app like *Urls*, *UserDefault keys*, *Alert Messages*.
- **Structs.swift**, which contains common `struct`s used in the app.
- **AppDelegate.swift**, the starting point for your app, logically placed at the top of the folder structure.
- **API**, the API folder contains a subfolder for *APIRequest* and *Models* because they’re closely related to the APIs themselves.
- Then almost all the other folders are specific UIs. Every **UI** has its own **Swift class** file and a **XIB**. Any UI that’s part of another UI, such as the ***“Search Popover Controller”*** which is part of the ***"Appointment View Controller"***, is structured as a subfolder.
- In the **Supporting Files** you can see a few subdirectories, for extensions for instance, 
- Organize classes and **XIB**s by functionality, nested by sub-functionality or sub-UI.
- Non-UI, framework-like code has its own folders, like **API**.
Models have their own folders.
- Generic UI code, like subclassing **UINavigationController**, goes lower down on the UI-folder list.
- Support files, like **extensions**, **utilities** and **non-source-controlled libraries** go in their own folder.
