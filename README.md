# JioMoney_Android_SDK
JioMoney Android SDK enables merchants to integrate JioMoney wallet inside their android application using 3 easy steps. And SDK can be  understood by developer with basic android knowledge. 
# Requirements 
SDK supports minimum version of android API LEVEL 10 that is version 2.3.3.
# Installation
<pre>Import/add library into your project. You can do this in following two ways.
<b>a.</b>	Add project dependency inside build.gradle file of app 
Add .arr file into libs directory of project
Add below code to project level build.gradle

allprojects {
   repositories {
      jcenter()
      flatDir {
        dirs 'libs'
      }
   }
}
Add dependency to app level of build.gradle

compile(name:'jm_sdk', ext:'aar')

Then synchronise the project.
<b>b.</b>	Go to project structure of android studio and add file dependency in dependencies tab.
Below is the link for instructions for importing .aar file into Android Studio
<pre>https://stackoverflow.com/questions/16682847/how-to-manually-include-external-aar-package-using-new-gradle-android-build-syst
</pre></br>
# Usage
<b>1.</b>Initialize the SDK with below code.
JMPaymentConfig.getInstance()
                .setJMEnvironment(JMEnvironment.PRE_PROD)
                .setClientId(“client_id”)
                .setMerchantId("merchant_id")
                .setReturnUrl("return url")
                .setVersion(JMPaymentConfig.JMVersion.VERSION2_0)
                .setCurrency(JMPaymentConfig.JMCurrency.INR)
                .setEnableLog(true)
                .setAutoSubmitOTP(true)
                .init(this);
		
<b>2.</b>Set necessary parameters Create the instance of JMPayment model with basic values(mandatory)
JMPayment payment = new JMPayment(“extRefNo",
        “timestamp”,
        amount,
        mobile,
        “checksumHash”);
	
<b>3.</b>Call payment method to process the payment.
JMPaymentService.getInstance().makePayment(context, JMPayment, new JMPaymentTransactionCallback() {
    @Override
    public void onResponse(final JMPaymentResponse JMPaymentResponse, String rawResponse) {
        Log.d(TAG, "response: " + JMPaymentResponse.toString());      
    }
    @Override
    public void onError(int errorCode, final String error) {
    }
});
# More Description
Parameter Details:
<b>client_id:-</b> This is provided by Integration team for integration and testing in Sandbox environment.
<b>merchant_id:-</b> This is provided by Integration team for integration and testing in Sandbox environment.
<b>return_url:-</b> This is the parameter where you will get the response of the transaction after completion. This URL must be hosted at merchant’s end.
<b>version:-</b> 2.0 needs to be passed as this SDK supports version 2.0 
<b>currency:-</b> INR should be passed, as the supported currency is INR(as of now)
<b>setEnableLog:-</b> This method is used to see the logs, in the prod environment.The value must be set to false.
<b>extRefNo:-</b> This is a unique ref number generated by merchant which will be used as reference in-case of customer queries.
<b>timestamp:-</b> This is used to maintain merchant transaction time at JioMoney side
<b>amount:-</b> This is actual parameter of type double for payment
<b>mobile:-</b> This parameter is the valid mobile number which will be displayed in the payment screen before login
<b>checksum_hash:-</b> This is the value generated at server side to make transaction temper free.
The checksum logic is shared inside integration document.
<b>makePayment:</b> - Method of JMpaymentService class is used to initiate transaction which include below parameters
       context: - as first parameter of type android context
       JMPayment: - This must be initialized with basic parameters before calling make payment
       JMPaymentTransactionCallback: - contains callback method for success and failure case.

<b>Note:-</b> the parameters mentioned in the request must be same while generating checksum hash
1.	Checksum generation logic for version 2.0
a.	If you are setting product description and UDF’s in JMPayment instance use below checksum generation logic
Clientid|Amount|Extref|Channel|MerchantId|Token|ReturnUrl|TxnTimeStamp|TxnType|subscriber.mobilenumber|productdescription|UDF1|UDF2|UDF3|UDF4|UDF5

b.	If you are not setting product description and UDF’s in JMPayment instance use below checksum generation logic
Clientid|Amount|Extref|Channel|MerchantId|Token|ReturnUrl|TxnTimeStamp|TxnType|subscriber.mobilenumber

2.	If you want to add more parameters mentioned in Integration doc, there is a provision to set those values using setter method of JMPayment class.
