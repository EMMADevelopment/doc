# iOS INSTALLATION

1. *First we’ll need to override the ‘UnityAppController’ class with our own version. To do this we need to create in the Assets folder of your Unity project a ‘Plugins’ folder and inside this folder add a ‘iOS’ folder. So your structure needs to look like this*:

    Assets
        Plugins
            iOS

*Make sure you name everything correct otherwise it will not work. Unity checks if these folders exists and copy all their contents to the Xcode project*.

2. *Copy all files from eMMa iOS SDK*:

- eMMa.h
- eMMaDefines.h
- eMMa-Universal-static.a 

3. *Now you need to create a file in the iOS folder like “eMMaAppController.mm”. You can call yours whatever you want it to be*.

4. **Now the important part**: *don’t edit the file in this Xcode project, because it will be overwritten every time you do a new Unity build! Instead go to Unity and double-click on the file (it will also open in Xcode). Now we can override Unity’s AppController class with the following code*.

#import <UIKit/UIKit.h>
#import “UnityAppController.h”
#import “UI/UnityView.h”
#import “UI/UnityViewControllerBase.h”
#import “eMMa.h”

 
@interface eMMaAppController : UnityAppController
 
(void)createViewHierarchyImpl;
 
@end
 
@implementation eMMaAppController
 
(void)createViewHierarchyImpl {
 
		# Start sessiom with eMMa and your Session Key
		[eMMa starteMMaSession:@“YOURSESSIONKEY”];

		# Also, if needed, you can track location using 
		[eMMa trackLocation];

		# Create view hierarchy
    _rootController = [[UIViewController alloc] init];
    _rootView = [[UIView alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
    _rootController.view = _rootView;

    	# Recover Unity focus
		[self.view addSubview:GetAppController().unityView];
    GetAppController().unityView.frame = self.view.frame;

}
 
@end
 
IMPL_APP_CONTROLLER_SUBCLASS(eMMaAppController)

