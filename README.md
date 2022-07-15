# CROPUS ANDROID SDK
![version](https://img.shields.io/badge/version-v1.1.0-blue)

The Cropus Android SDK is a real-time signature capture and crop solution for Android.

Features available are
- Signature Capture
- Manual Crop
- White Background Removal (Transparent Background , allowing seamless overlaying of the signature on another document)

**Find the changelog and release history at [Here](CHANGELOG.md)**

‼ ATTENTION ‼ → BREAKING CHANGE introduced at Cropus SDK `v1.1.0`. We have introduced a new license format. If you are using versions prior to `v1.1.0` and intend to update to v1.1.0, contact `frslabs@support.com` for an updated license.

# Table Of Content

- [Prerequisite](#prerequisite)
- [Android SDK Requirements](#android-sdk-requirements)
- [Download](#download)
  - [Using maven repository](#using-maven-repository)
- [Setup](#setup)
  - [Permissions](#permissions)
- [Quick Start](#quick-start)
  - [Invoking the Cropus SDK](#invoking-the-cropus-sdk)
- [Cropus SDK Result](#cropus-sdk-result)
- [Cropus SDK Error Codes](#cropus-sdk-error-codes)
- [Cropus SDK Parameters](#cropus-sdk-parameters)
- [Help](#help)

## Prerequisite

You will need a valid license to use the Cropus Android SDK, which can be obtained by contacting `support@frslabs.com` . 

Once you have the license , follow the below instructions for a successful integration of Cropus Android SDK onto your Android Application.

## Android SDK Requirements

**Minimum SDK Version** -  **23**

**Compile SDK Version** - **32**

## Download

#### Using maven repository

Add the following code to your `project` level `build.gradle` file

```groovy
allprojects { 
    repositories { 
       maven { 
            // Maven Url and Credentials for Cropus SDK. 
            url "https://cropus-android.repo.frslabs.space/"                  
            credentials { 
                   username 'repo-username' 
                   password 'repo-password' 
            }
       }
        
    }
}
```

After that, add the following code to your `app` level `build.gradle` file

```groovy
// ...

  compileOptions {
      
       sourceCompatibility = 1.8
       targetCompatibility = 1.8
       
       //Add below line only if minSdkVersion is < 24 
       coreLibraryDesugaringEnabled true
       
  }

// ...
```

And then, add the dependencies
```groovy

// ...

dependencies {
    /* Standard AndroidX Library Dependencies */ 
    implementation 'com.google.android.material:material:1.1.0'
    implementation 'androidx.appcompat:appcompat:1.1.0'
    implementation 'androidx.constraintlayout:constraintlayout:1.1.3'
   
    //REQUIRED - Cropus SDK Dependency
    implementation 'com.frslabs.android.sdk:cropus:1.1.0'
    
    //Add below line only if minSdkVersion is < 24 
    coreLibraryDesugaring 'com.android.tools:desugar_jdk_libs:1.0.10'
    
}
```

## Setup

#### Permissions

Cropus SDK uses the required permissions and features
```xml

    <!--Permissions-->
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.VIBRATE" />
    
    <!--Features-->
    <uses-feature android:name="android.hardware.camera" />
    <uses-feature android:name="android.hardware.camera.autofocus" />

```

## Quick Start

#### Invoking the Cropus SDK

Initialize the `Cropus` instance with the appropriate configurations. 
Call `start` on the instance to invoke the SDK.
Handle the result by extending the `CropusResultCallback`

```java
public class MainActivity extends AppCompatActivity implements CropusResultCallback {

    // ...

    /* Enter the Cropus SDK license key here */
    private String CROPUS_LICENSE_KEY = "ENTER_YOUR_LICENSE_KEY_HERE";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        Button callSdk = findViewById(R.id.call_sdk);
        callSdk.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                /* Invoke the Cropus SDK */
                invokeCropusSDK();
            }
        });
    }

    private void invokeCropusSDK() {
    
        //Initialize the Cropus SDK Config object with the appropriate configurations
        CropusConfig cropusConfig = new CropusConfig.Builder()
                .setLicenseKey(CROPUS_LICENSE_KEY)
                .setOutputImageFormat("jpg")    //Output image format either jpg or png by default result will be in jpg format. 
                .setOutputImageResolution("both") // Output image resolution either both,low,high by default result is in high resoltion image.
                .build();

        //Call the Cropus SDK 
        Cropus cropus = new Cropus(cropusConfig);
        cropus.start(this, this);
    }
    
    @Override
    public void onCropusSuccess(CropusResult cropusResult) {
        //Handle the Cropus SDK Result Here
        Toast.makeText(this, cropusResult.getImagePath().getPath(), Toast.LENGTH_LONG).show();
    }

    @Override
    public void onCropusFailure(int errorCode) {
        /* Handle the Cropus SDK error here */
        Toast.makeText(this, "Error: "+ errorCode, Toast.LENGTH_SHORT).show();
    }
    
    // ...

}
```
## Cropus SDK Result

The result is obtained through the `CropusResult` object

Given below are the public methods in brief.
<div>
<table style="width:100%">
 <tr>
 <th bgcolor="#F1F1F1" colspan="3">Public Methods</th>
 </tr>
 <tr>
 <td>Uri</td>
 <td>getImagePath()</td>
 <td>Returns the Signature Image Path as Uri</td>
 </tr>
   <tr>
 <td>Uri</td>
 <td>getImagePathHighres()</td>
 <td>Returns the Signature high resolution Image Path as Uri</td>
 </tr>
   <tr>
 <td>Uri</td>
 <td>getImagePathLowres()</td>
 <td>Returns the Signature lower resolution Image Path as Uri</td>
 </tr>
</table>
</div>

## Cropus SDK Error Codes

Error codes and their meaning are tabulated below

| Label          | Code |Message                 |
| -------------- | ----- |---------------------- |
|ERROR_CODE_CAM_PERMISSION | 803 | Required permissions for Cropus SDK were not granted |
|ERROR_CODE_INTERRUPTED | 804 | Cropus SDK Interrupted |
|ERROR_CODE_EXPIRED_LICENCE | 805 | Cropus SDK License has expired |
|ERROR_CODE_INVALID_LICENCE | 806 | Invalid Cropus SDK License |
|ERROR_CODE_INVALID_CONFIG | 807 | Invalid Cropus SDK Config |
|ERROR_CODE_FILE_IO | 809 | Error Saving File to Disk |

## Cropus SDK Parameters

- `setLicenseKey(String cropusLicenseKey)`   ***(Required)***
  
  Accepts the Cropus SDK licence key as a `String`

## Help
For any queries/feedback , contact us at `support@frslabs.com` 
