
##CocoaPod

ref1 : https://www.kodeco.com/7076593-cocoapods-tutorial-for-swift-getting-started
<br>
ref2 : https://guides.cocoapods.org/


CocoaPods uses Ruby, which ships with all versions of macOS X since version 10.7.


> sudo gem install cocoapods

---

Finally, enter this command in Terminal to complete the setup:

> pod setup --verbose

This process takes a few minutes because it clones the CocoaPods Master Specs repository into ~/.cocoapods/ on your computer.

The **verbose** option logs progress as the process runs, allowing you to watch the process instead of seeing a seemingly “frozen” screen.

---

Next, enter the following command:

> pod init

This creates a Podfile for your project.

Configure your podfile.

Finally, do a “pod install” in your terminal and wait for it to install the pod.

> pod install

From now on you should open the project using the **.xcworkspace** in order to properly build and use the pods in your project. 

