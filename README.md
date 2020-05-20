# Espresso tips

* Don't use `pressBack()` to close the keyboard as it can be inconsistent - use `closeSoftKeyboard()` instead
* `pressBack` can be called on any screen so it's good to pair with an assertion to check you're where you think you are before calling
* It's good to keep the above in mind for pretty much any kind of action that might work on multiple screens - make sure the test verifies it is in the right place before it tries to do something
* Don't close dialogs by clicking on "OK" as different API versions use different capitalisations - use `withId(android.R.id.button1)` instead
* Always use `RecyclerViewActions` instead of trying to manually scroll to items on screen
* `Handler#postDelayed` (or just `post`) in code can cause flakes in Espresso - wrap Handler in an interface you can swap out in test with something integrates with `CountingIdlingResource` to make Espresso aware of posted UI jobs
* `LiveData#post` runs into the same problems as above - Switching out ArchTaskExecutor with `CountingTaskExecutorRule` and then integrating that with an `IdlingResource` can solve this for you
