Pets Shared Device Demo
=======================

A demo app built as a proof-of-concept for Pets At Home.

This app uses the Microsoft Authentication Library (MSAL) to prove we easily detect whether a device has been set to "shared mode".

See this article on [MSAL Android shared devices](https://docs.microsoft.com/en-us/azure/active-directory/develop/msal-android-shared-devices).

This app was built with the assistance of [this tutorial](https://docs.microsoft.com/en-us/azure/active-directory/develop/tutorial-v2-android).

Note, that the method to generate the signature hash didn't work for me, see this 
[stack overflow answer](https://stackoverflow.com/questions/58757706/unable-to-create-singleaccountpublicclientapplication-for-azure-ad-on-android) for details.

This results in this code block outputting in logcat the real hash value to use in the Azure AD configuration:

````
try {
    PackageInfo info = getPackageManager().getPackageInfo(
            "uk.co.thepricingpeople.petsshareddevicedemo",
            PackageManager.GET_SIGNATURES
    );
    for (Signature signature : info.signatures) {
        MessageDigest md;
        md = MessageDigest.getInstance("SHA");
        md.update(signature.toByteArray());
        String something = new String(Base64.encode(md.digest(), 0));
        //String something = new String(Base64.encodeBytes(md.digest()));
        Log.e("KeyHash", something);
    }
} catch (PackageManager.NameNotFoundException e1) {
    Log.e("name not found", e1.toString());
} catch (NoSuchAlgorithmException e) {
    Log.e("no such an algorithm", e.toString());
} catch (Exception e) {
    Log.e("exception", e.toString());
}
````