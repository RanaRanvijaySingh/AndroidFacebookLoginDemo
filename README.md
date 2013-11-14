AndroidFacebookLoginDemo
========================
This application a demo to show how to use the facebook login as to login in to your app .
<br/>There are a particular set of steps that you need to follow . These steps are well defined by the facebook developers .:
<br/>http://developers.facebook.com/docs/android/getting-started/facebook-sdk-for-android/
<br/>
<br/>I will assume you have the facebook sdk and you have successfully installed it .
<br/>Step 1: Generate you Hash key .
<br/>Step 2: Create new application .
<br/>Step 3: Register you app with the facebook using hash key and get the application ID.
<br/>Step 4: Fill the details of the “Android Native Application”.
<br/>Step 5: Create a string with a application id in “string.xml”
<br/>Step 6: Add the permission to your manifest file .
<br/>Step 7: Create the main java file. “MainActivity”
____________________________________________________________________________________________________________________________________________________________

Step 1: Generate you Hash key .

Give this in the terminal : 
keytool -exportcert -alias androiddebugkey -keystore ~/.android/debug.keystore | openssl sha1 -binary | openssl base64

____________________________________________________________________________________________________________________________________________________________
Step 2: Create new application .

____________________________________________________________________________________________________________________________________________________________
Step 3: Register you app with the facebook using hash key and get the application ID.

____________________________________________________________________________________________________________________________________________________________
Step 4: Fill the details of the “Android Native Application”.
	package Name	:com.webonise.friendsfinder 
	class name 		:com.webonise.friendsfinder.MainActivity
	key hash 		:7Kdj2tzXlr8rEa92imxbwKYTl0Q

____________________________________________________________________________________________________________________________________________________________
Step 5: Create a string with a application id in “string.xml”
 	
<string name="app_id">516430311784548</string>

____________________________________________________________________________________________________________________________________________________________	
Step 6: Add the permission to your manifest file .

    <uses-permission android:name="android.permission.INTERNET"/>

    <application
       ...
        <activity
           ...
            <intent-filter>
               ...
            </intent-filter>
           
        </activity>
         <meta-data android:name="com.facebook.sdk.ApplicationId" android:value="@string/app_id"/>
         <activity android:name="com.facebook.LoginActivity" android:label="@string/app_name"></activity>
    </application>

</manifest>

____________________________________________________________________________________________________________________________________________________________
Step 7: Create the main java file. “MainActivity”

public class MainActivity extends Activity {

	@Override
	public void onCreate(Bundle savedInstanceState) {
...

		// start Facebook Login
		Session.openActiveSession(this, true, new Session.StatusCallback() {

			// callback when session changes state
			@Override
			public void call(Session session, SessionState state,
					Exception exception) {
				if (session.isOpened()) {

					// make request to the /me API
					Request.executeMeRequestAsync(session,
							new Request.GraphUserCallback() {

								// callback after Graph API response with user
								// object
								@Override
								public void onCompleted(GraphUser user,
										Response response) {
									if (user != null) {
										TextView textViewWelcome = (TextView) findViewById(R.id.welcome);
										textViewWelcome.setText("Hello "
												+ user.getName() + "!");
									}
								}
							});
				}
			}
		});
	}

	@Override
	public void onActivityResult(int requestCode, int resultCode, Intent data) {
		super.onActivityResult(requestCode, resultCode, data);
		Session.getActiveSession().onActivityResult(this, requestCode,
				resultCode, data);
	}

}


