## Notes
* You can use Toolbar in two way
    1. Using Toolbar as ActionBar
    2. Using stand-alone Toolbar

#### Basic Steps to Create a Navigation Drawer
1. Make sure you have this Gradle dependency added to your app/build.gradle file:
    ```java
    dependencies {
      compile 'com.android.support:design:25.2.0'
    }
    ```
1. Create a ```menu/drawer_view.xml``` file:
    ```xml
    <menu xmlns:android="http://schemas.android.com/apk/res/android">
        <group android:checkableBehavior="single">
            <item
                android:id="@+id/nav_first_fragment"
                android:icon="@drawable/ic_one"
                android:title="First" />
            <item
                android:id="@+id/nav_second_fragment"
                android:icon="@drawable/ic_two"
                android:title="Second" />
            <item android:title="Sub items">
              <menu>
                  <group android:checkableBehavior="single">
                      <item
                          android:icon="@drawable/ic_dashboard"
                          android:title="Sub item 1" />
                      <item
                          android:icon="@drawable/ic_forum"
                          android:title="Sub item 2" />
                  </group>
              </menu>
          </item>
        </group>
    </menu>
    ```
1. create a ```layout/nav_header.xml``` file
    ```xml
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="192dp"
        android:background="?attr/colorPrimaryDark"
        android:padding="16dp"
        android:theme="@style/ThemeOverlay.AppCompat.Dark"
        android:orientation="vertical"
        android:gravity="bottom">

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Header"
            android:textColor="@android:color/white"
            android:textAppearance="@style/TextAppearance.AppCompat.Body1"/>

    </LinearLayout>
    ```
1. edit ```activity_main.xml``` with the followings
    1. make ```android.support.v4.widget.DrawerLayout``` root of the view
    1. make vartical LinearLayout the first child of the root
        1. inside the LinearLayout make Toolbar first child. see toolbar section to learn more about it.
        1. after Toolbar add the view of the Activity. this could be FramLayout for fragment or any other Layout
    1. add ```android.support.design.widget.NavigationView``` as the last child of the root view. Also add the following attributes:
        * ```android:layout_gravity="start"``` -> to have slide effect
        * ```app:menu="@menu/drawer_view"```   -> to add menu
        * ```app:headerLayout="@layout/nav_header"``` -> to add header

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <!-- This DrawerLayout has two children at the root  -->
    <android.support.v4.widget.DrawerLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:id="@+id/drawer_layout"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <!-- This LinearLayout represents the contents of the screen  -->
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="vertical">

            <!-- The ActionBar displayed at the top -->
            <android.support.v7.widget.Toolbar
                xmlns:android="http://schemas.android.com/apk/res/android"
                xmlns:app="http://schemas.android.com/apk/res-auto"
                android:id="@+id/toolbar"
                android:layout_height="wrap_content"
                android:layout_width="match_parent"
                android:fitsSystemWindows="true"
                app:title="Alondo"
                android:minHeight="?attr/actionBarSize"
                app:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
                android:background="?attr/colorPrimaryDark">
            </android.support.v7.widget.Toolbar>

            <!-- The main content view where fragments are loaded -->
            <!-- this can also be any other layout-->
            <FrameLayout
                android:id="@+id/flContent"
                app:layout_behavior="@string/appbar_scrolling_view_behavior"
                android:layout_width="match_parent"
                android:layout_height="match_parent" />
        </LinearLayout>

        <!-- The navigation drawer that comes from the left -->
        <!-- Note that `android:layout_gravity` needs to be set to 'start' -->
        <android.support.design.widget.NavigationView
            android:id="@+id/nvView"
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:layout_gravity="start"
            android:background="@android:color/white"
            app:menu="@menu/drawer_view"
            app:headerLayout="@layout/nav_header"/>
    </android.support.v4.widget.DrawerLayout>
    ```
1. By now the navigation drawer is working. but we need to add toggle button. to add toggle button do the followings in the Activity class:
    1. add class fields
       ```java
       private DrawerLayout mDrawer;
       private Toolbar toolbar;
       private NavigationView nvDrawer;

       // Make sure to be using android.support.v7.app.ActionBarDrawerToggle version.
       // The android.support.v4.app.ActionBarDrawerToggle has been deprecated.
       private ActionBarDrawerToggle drawerToggle;
       ```
    2. Override the ```onCreate``` method
        ```java
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            // Set a Toolbar to replace the ActionBar.
            toolbar = (Toolbar) findViewById(R.id.toolbar);
            setSupportActionBar(toolbar);

            // Find our drawer view
            mDrawer = (DrawerLayout) findViewById(R.id.drawer_layout);
            nvDrawer = (NavigationView) findViewById(R.id.nvView);

            //adding toggle button
            drawerToggle = new ActionBarDrawerToggle(this, mDrawer, toolbar, R.string.drawer_open,  R.string.drawer_close);
            mDrawer.addDrawerListener(drawerToggle);

        }
        ```
    3. Override the ```onPostCreate``` method with right signature
    ```java
    //this method is needed to show the toggle button
    // `onPostCreate` called when activity start-up is complete after `onStart()`
    // NOTE 1: Make sure to override the method with only a single `Bundle` argument
    // Note 2: Make sure you implement the correct `onPostCreate(Bundle savedInstanceState)` method.
    // There are 2 signatures and only `onPostCreate(Bundle state)` shows the hamburger icon.
    @Override
    protected void onPostCreate(Bundle savedInstanceState) {
        super.onPostCreate(savedInstanceState);
        // Sync the toggle state after onRestoreInstanceState has occurred.
        drawerToggle.syncState();
    }
    ```
    4. Override the ```onConfigurationChanged``` method
        ```java
        @Override
        public void onConfigurationChanged(Configuration newConfig) {
            super.onConfigurationChanged(newConfig);
            // Pass any configuration change to the drawer toggles
            drawerToggle.onConfigurationChanged(newConfig);
        }
        ```
    1. Override ```onOptionsItemSelected``` the method and allow the ActionBarToggle to handle the events.
        ```java
        @Override
        public boolean onOptionsItemSelected(MenuItem item) {
            if (drawerToggle.onOptionsItemSelected(item)) {
                return true;
            }

            return super.onOptionsItemSelected(item);
        }
        ```
1. Now you have Navigation Drawer with toggle button. But we need to add ClickListener. For this do the followings:
    1. add the snippet of code in the ```onCreate``` method of the activity class
        ```java
        nvDrawer.setNavigationItemSelectedListener((MenuItem menuItem)->{
                //Add logic of your OnCliclListener Here
                Log.d("MenuItem", "id "+menuItem.getItemId());

                // Highlight the selected item has been done by NavigationView
                menuItem.setChecked(true);
                // Set action bar title
                getSupportActionBar().setTitle(menuItem.getTitle());
                // Close the navigation drawer
                mDrawer.closeDrawers();
                return true;
            });
        ```
    2. If you get error using Lambda Expression follow this [link](https://developer.android.com/studio/write/java8-support.html)
    3. ```Limitations```: The current version of the design support library does come with its limitations. The main issue is with the system that highlights the current item in the navigation menu. The itemBackground attribute for the NavigationView does not handle the checked state of the item correctly: somehow either all items are highlighted or none of them are. This makes this attribute basically unusable for most apps.


## Toolbar
* [App Bar](https://developer.android.com/training/appbar/index.html)
* [Using-the-App-Toolbar](https://github.com/codepath/android_guides/wiki/Using-the-App-Toolbar)

## navigation Drawer
* [Fragment-Navigation-Drawer](https://github.com/codepath/android_guides/wiki/Fragment-Navigation-Drawer)
* [Creating a Navigation Drawer](https://developer.android.com/training/implementing-navigation/nav-drawer.html)
