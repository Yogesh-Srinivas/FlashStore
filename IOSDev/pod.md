
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



###pod install

This is to be used the first time you want to retrieve the pods for the project, but also every time you edit your Podfile to add, update or remove a pod.

Every time the pod install command is run — and downloads and install new pods — it writes the version it has installed, for each pods, in the Podfile.lock file. This file keeps track of the installed version of each pod and locks those versions.
When you run pod install, it only resolves dependencies for pods that are not already listed in the Podfile.lock.

For pods listed in the Podfile.lock, it downloads the explicit version listed in the Podfile.lock without trying to check if a newer version is available

For pods not listed in the Podfile.lock yet, it searches for the version that matches what is described in the Podfile (like in pod 'MyPod', '~>1.2')

###pod outdated
When you run pod outdated, CocoaPods will list all pods which have newer versions than the ones listed in the Podfile.lock (the versions currently installed for each pod). This means that if you run pod update PODNAME on those pods, they will be updated — as long as the new version still matches the restrictions like pod 'MyPod', '~>x.y' set in your Podfile.


###pod update
When you run pod update PODNAME, CocoaPods will try to find an updated version of the pod PODNAME, without taking into account the version listed in Podfile.lock. It will update the pod to the latest version possible (as long as it matches the version restrictions in your Podfile).

If you run pod update with no pod name, CocoaPods will update every pod listed in your Podfile to the latest version possible.