## Notes
1. You can have multiple layout folder. ```layout``` for portrait mode and ```layout-land``` for landscape mode.
1. Storing data on screen rotation
    1. __Activity:__ use ```onSaveInstanceState(...)``` method to store data in bundle and use ```onRestoreInstanceState(...)``` to restore the data from the bundle.
    1. __Fragment:__ use ```onSaveInstanceState(...)``` method to store data in bundle and use ```onViewStateRestored(...)``` to restore the data from the bundle.
