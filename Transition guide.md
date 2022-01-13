# Transition to PhotoPay v8.0.0

### The SDK now includes only `PhotoPay` and `BlinkInput` recognizers
- If you want to use `BlinkID` recognizers, you should import `BlinkID` SDK ([more here](https://github.com/BlinkID/blinkid-android#-sdk-integration)).

#### Adding _BlinkID_ dependency

- In your `build.gradle`, add _BlinkID_ maven repository to repositories list

	```
	repositories {
	    maven { url 'https://maven.microblink.com' }
	}
	```

- Add _BlinkID_ as a dependency and make sure `transitive` is set to true

	```
	dependencies {
	    implementation('com.microblink:blinkid:5.15.0@aar') {
	        transitive = true
	    }
	}
	```
### Repackaging of all classes
- The root package has been renamed from `com.microblink` to `com.microblink.photopay`. To implement this change, you should simply replace all occurrences of `com.microblink` with `com.microblink.photopay`. Most of these changes should be on imports of `PhotoPay` SDK Java classes.

### Combining of PhotoPay and BlinkID
- In `PhotoPay v8.0.0` we've removed `BlinkID` recognizers from the SDK. You should use `BlinkID` SDK to use `BlinkID` recognizers. If you're using both `PhotoPay` and `BlinkID` recognizers in you app, be careful from which SDK you're importing Java classes.
- For example, the recommended way of setting a license is through extending the Android Application class. If you would want to set the license for both `BlinkID` and `PhotoPay`, you should do it like this:

	```java
	    public class MyApplication extends Application {
	        @Override
	        public void onCreate() {
	        	com.microblink.MicroblinkSDK.setLicenseFile("path/to/BlinkID/license/file/within/assets/dir", this);
	            com.microblink.photopay.MicroblinkSDK.setLicenseFile("path/to/PhotoPay/license/file/within/assets/dir", this);
	        }
	    }
	```


### Minor API changes

- Getters for some of the fields have been renamed according to the guidelines we have for all of our recognizers.
	- For example, `getIban` instead of `getIBAN`, `getBic` instead of `getBIC`, `getBlz` instead of `getBLZ`, etc.
- To implement this change, replace the old getter names with new ones.

### Other questions
- If you have any other questions, please check our [public documentation ](https://github.com/PhotoPay/photopay-android) or contact us.