# Espresso tips

* Espresso can become messy very fast - using the Page Object pattern is pretty helpful for writing readable (and maintainable tests)
* Don't use `pressBack()` to close the keyboard as it can be inconsistent - use `closeSoftKeyboard()` instead
* `pressBack` can be called on any screen so it's good to pair with an assertion to check you're where you think you are before calling
* It's good to keep the above in mind for pretty much any kind of action that might work on multiple screens - make sure the test verifies it is in the right place before it tries to do something
* Don't close dialogs by clicking on "OK" as different API versions use different capitalisations - use `withId(android.R.id.button1)` instead
* Always use `RecyclerViewActions` instead of trying to manually scroll to items on screen
* `Handler#postDelayed` (or just `post`) in code can cause flakes in Espresso due to it not waiting for async work to execute before making assertions - wrap Handler in an interface you can swap out in test with something that integrates with `CountingIdlingResource` to make Espresso aware of posted UI jobs. Or, use coroutines and solve the problem there instead.
* `LiveData#post` runs into the same problems as above - Switching out ArchTaskExecutor with `CountingTaskExecutorRule` and then integrating that with an `IdlingResource` can solve this for you
* Using Coroutines will also cause flakes (like the above two points) - I'm not actually sure what the best way to deal with this is. One course of action is to build an abstraction around coroutines and then swap that out for a "counting" version (like about) to integrate with `IdlingResource`. There is hopefully something smarter that could be done by swapping out coroutine dispatchers in tests.
* If you really can't solve a timing issue (specifically Espresso failing before some transition occurs/data loads etc) then using a wait helper that repeats the asssertion until it passes (and fails after a given timeout) is probably a good option.
