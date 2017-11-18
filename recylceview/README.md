## Note

* Using a RecyclerView has the following key steps:

    1. Add RecyclerView support library to the gradle build file
    2. Define a model class to use as the data source
    3. Add a RecyclerView to your activity to display the items
    4. Create a custom row layout XML file to visualize the item
    5. Create a RecyclerView.Adapter and ViewHolder to render the item
    6. Bind the adapter to the data source to populate the RecyclerView

* Implementing Different Layout for recycleview
    1. create multiple layout file
    1. create a parent ViewHolder class and add a variable to store *type* and a method to get the *type*
    1. create multiple subclass of the parent ViewHolder class in the Adapter class
    1. override ```getItemViewType(int position)``` method in Adapter class
    1. in the ```onCreateViewHolder(ViewGroup parent, int viewType)``` method make use of ```viewType``` variable when returning ViewHolder and stor the ```viewType``` in the ViewHolder class
    1. in the ```onBindViewHolder(ViewHolder holder, int position)``` method Bind the appropriate views according to the type of the ViewHolder
    1. Now you have multiple types of row in a single recycleview. enjoy...... :thumbsup:  


* Different types of Layout Manager
    * [LinearLayoutManager](https://developer.android.com/reference/android/support/v7/widget/LinearLayoutManager.html)
    * [StaggeredGridLayoutManager](https://developer.android.com/reference/android/support/v7/widget/StaggeredGridLayoutManager.html)
    * [GridLayoutManager](https://developer.android.com/reference/android/support/v7/widget/GridLayoutManager.html)
    * [WearableLinearLayoutManager](https://developer.android.com/reference/android/support/wear/widget/WearableLinearLayoutManager.html)

## Links

* [Using-the-RecyclerView](https://github.com/codepath/android_guides/wiki/Using-the-RecyclerView)
