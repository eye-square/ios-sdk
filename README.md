# ios-sdk

## Installation


### Start a task

```swift
let storyboard = UIStoryboard(name: "Task", bundle: Bundle(identifier: "com.eyesquare.InContextSDK"))
let controller = storyboard.instantiateViewController(withIdentifier: "TaskViewController") as! TaskViewController

controller.delegate = self
controller.data = TaskData(ProjectID: "fallback-demo", SubjectGroupID: "fb", UserHash: "carlos")

self.present(controller, animated: false, completion: nil)
```

```objective-c
Storyboard*         storyboard = [UIStoryboard storyboardWithName:@"Task" bundle: [NSBundle bundleWithIdentifier:@"com.eyesquare.frm"]];
TaskViewController* controller = (TaskViewController*)[storyboard instantiateViewControllerWithIdentifier:@"TaskViewController"];

[self presentViewController:controller animated:NO completion:nil];
```
