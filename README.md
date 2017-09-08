# ios-sdk

The `-dev` version of the framework is a universal build containing both the simulator and device architectures. Since the AppStore won't accept universal fat binaries there's a second version built only for devices. Alternatively you could use just `-dev` framework and combine it with a script to strip the unused architectures before archiving.

## Installation

Download InContextSDK.framework.zip, extract it and add InContextSDK.framework to your project. It should appear listed under *General > Linked Frameworks and Libraries* and *General > Embedded Binaries*.

For objective-c projects, also set *Build Settings > Always Embed Swift Standard Libraries* to `Yes`

Finally, frameworks should be added to the path already, in case they're not add `@executable_path/Frameworks` to *Build Settings > Runpath Search Paths*.

## Usage

Starting a task and and retrieving its results works similarly for both for swift and objective-c.
- A task is started by instantiating the `TaskViewController` from the `TaskStoryboard` and showing it.
- ProjectID, SubjectGroupID and UserHash are set using the `data` property of the task controller.
- Finally, to receive the task end event and success, we can use the `delegate` property of the task controller. The listener needs to implement the `TaskDelegate` protocol. In the examples, we've used the main ViewController for that purpose.

### possible success values:

success is an integer and represents the result of the task.

* -1: a technical error occured, the subject couldn't start/get through the survey. This is often caused by wrongly passed ProjectIds and or SubjectGroupIds
* 0: the success criteria of the ad/subjectGroup wasn't met
* 1: the success criteria of the ad/subjectGroup was met
* 2: the user canceled the run early

#### Swift

```swift
import UIKit
import InContextSDK


class ViewController: UIViewController, TaskDelegate {

	...

	//---------------------------------------------------------------------------
	@IBAction func onButtonPressed(_ sender: UIButton) {

		// instantiate the controller
		let storyboard = UIStoryboard(name: "Task", bundle: Bundle(identifier: "com.eyesquare.InContextSDK"))
		let controller = storyboard.instantiateViewController(withIdentifier: "TaskViewController") as! TaskViewController

		// define task variables
		let taskData = TaskData(ProjectID: "2017-05_DE030_j2vyo5nj", SubjectGroupID: "fbpers", UserHash: "1234")

		// set data and task end listener
		controller.delegate = self
		controller.data = taskData

		// start task
		self.present(controller, animated: false, completion: nil)
	}


	//---------------------------------------------------------------------------
	func taskCallback(_ success: Int){
		// handle success
	}
}
```

#### Objective-c

##### ViewController.h:
```objective-c
#import <UIKit/UIKit.h>
#import "InContextSDK/InContextSDK-Swift.h"

@interface ViewController : UIViewController <TaskDelegate>

@end
```

##### ViewController.m:

```objective-c
#import "ViewController.h"

@interface ViewController ()
@end

@implementation ViewController

...

//---------------------------------------------------------------------------
- (IBAction)onButtonPressed:(id)sender {

	// instantiate the controller
	UIStoryboard*       storyboard = [UIStoryboard storyboardWithName:@"Task" bundle: [NSBundle bundleWithIdentifier:@"com.eyesquare.InContextSDK"]];
	TaskViewController* controller = (TaskViewController*)[storyboard instantiateViewControllerWithIdentifier:@"TaskViewController"];

	// define task variables
	TaskData* taskData = [[TaskData alloc] initWithProjectID:@"2017-05_DE030_j2vyo5nj" SubjectGroupID:@"fbpers" UserHash:@"1234"];
	
	// set data and task end listener
	controller.delegate = self;
	controller.data     = taskData;
	
	// start task
	[self presentViewController:controller animated:NO completion:nil];
}


//---------------------------------------------------------------------------
- (void)taskCallback :(int)success {
	// handle success
}
@end
```

