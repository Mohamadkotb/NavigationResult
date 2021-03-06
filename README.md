# NavigationResult [![](https://api.bintray.com/packages/m4hdi/NavigationResult/NavigationResult/images/download.svg)](https://bintray.com/beta/#/m4hdi/NavigationResult?tab=packages)
```startActivityForResult``` but for fragments! (Addon for Jetpack's [Navigation component](https://developer.android.com/guide/navigation/navigation-getting-started)).  
🗒 You can read a blog post about this library from [here](https://android.jlelse.eu/navigateup-with-bundle-c595ff6d91ba).
## How to use
1. Add NavigationResult's dependency into your `build.gradle` file:
```groovy
dependencies {
    implementation "com.phelat:navigationresult:[latest_version]"
}
```
2. Extend your starting point fragment from `BundleFragment`
```kotlin
class FragmentA : BundleFragment() {
    ...
}
```
3. Navigate to destination fragment using `navigate` function:
```kotlin
navigate(FragmentADirections.fragmentAToFragmentB(), REQUEST_CODE)
// If you aren't using SafeArgs plugin, you can navigate with direction id
navigate(R.id.fragmentAToFragmentB, REQUEST_CODE)
```
4. Override `onFragmentResult` in your starting point fragment:
```kotlin
class FragmentA : BundleFragment() {

    override fun onFragmentResult(requestCode: Int, bundle: Bundle) {
        // Do whatever you want with result bundle from the destination fragment
    }
    
}
```
5. In your destination fragment use `navigateUp` extension function:
```kotlin
navigateUp(REQUEST_CODE, Bundle().apply {
    putBoolean("isLoginSuccessful", true)
})
```
6. Extend your activity from `FragmentResultActivity`:
```kotlin
class MainActivity : FragmentResultActivity() {
    // Return the id of your NavHostFragment
    override fun getNavHostFragmentId(): Int = R.id.nav_host_fragment
}
```
## Two ways for defining request code
1. You can pass request code through `navigate` function:
```kotlin
navigate(FragmentADirections.fragmentAToFragmentB(), REQUEST_CODE)
// OR
navigate(R.id.fragmentAToFragmentB, REQUEST_CODE)
```
2. You can also set request code in your navigation graph xml file:
```xml
<fragment
    android:id="@+id/fragment_a"
    android:name="com.sample.FragmentA">
    <action
        android:id="@+id/a_to_b"
        app:destination="@id/fragment_b">
        <argument
            android:name="fragment:requestCode"
            android:defaultValue="1000"
            app:argType="integer" />
    </action>
</fragment>
```
If you don't want to set default value for `fragment:requestCode`:
```kotlin
navigate(FragmentADirections.fragmentAToFragmentB(10000))
// OR
navigate(R.id.fragmentAToFragmentB, Bundle().apply {
    putInt("fragment:requestCode", 1000)
})
```
## Update NavHostFragment's id
You can update NavHostFragment's id at runtime using `updateNavHostFragmentId` function:
```kotlin
// This function is only available for FragmentResultActivity
updateNavHostFragmentId(R.id.anotherNavHostFragment)
```
## Navigate with request code from FragmentResultActivity
You can navigate with request code from FragmentResultActivity as you would do using BundleFragment.
## Navigate up from `<dialog/>` destinations
This library does support `<dialog/>` destinations and you can use them normally without doing anything extra.
## Sample
You can checkout the sample project for NavigationResult from [here](https://github.com/PHELAT/NavigationResult/tree/master/app)
