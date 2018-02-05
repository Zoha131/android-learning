## Notes

#### Steps to Create Basic Fragment

1. First create a layout resource called ```myfragment.xml``` to show in fragment
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical" android:layout_width="match_parent"
        android:layout_height="match_parent">

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Hello from fragment layout"
            android:id="@+id/fragment_text"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Fragment"
            android:id="@+id/frag_btn"/>

    </LinearLayout>
    ```

1. Create a subclass of ```Fragment``` and override the ```onCreateView``` method

    ```java
    public class FirstFragment extends Fragment {

        @Nullable
        @Override
        public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, Bundle savedInstanceState) {

            View view = inflater.inflate(R.layout.myfragment, container, false);
            Button btn = (Button) view.findViewById(R.id.frag_btn);
            return view;
        }
    }
    ```

1. Add *FrameLayout* to the ContentView of the Activity Class and give an id
    ```xml
    <FrameLayout
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="4"
            android:id="@+id/myfirstfragment"/>
    ```

1. Add the object of ```FirstFragment``` to the Activity class

    ```java
    public class MainActivity extends AppCompatActivity implements FirstFragment.OnSelectedListener {

        @Override
        protected void onCreate(Bundle savedInstanceState) {

            ........

            FirstFragment firstFragment = new FirstFragment();

            private FragmentManager fragmentManager = getFragmentManager();
            private FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();

            fragmentTransaction.add(R.id.myfirstfragment, firstFragment);
            fragmentTransaction.commit();

            .......

        }
    }
    ```

1. Enjoy..... :thumbsup:

#### sending data to a Fragment from Parent Activity
* the Fragment class has to getter and setter to flow Arguments.
In the parent Activity class you can call ```setArguments(Bundle args)``` method to pass arguments as Bundle. And in the ```onCreateView``` method of the Fragment class you can get the passed arguments by using ```getArguments()``` method.

* instead of using the default ```setArguments(Bundle args)``` method you can also use you custom method in Fragment class to pass data from parent class to Fragment


#### [Communicating with Other Fragments](https://developer.android.com/training/basics/fragments/communicating.html)

1. Define an Interface in the subclass of *Fragment*
    ```java
    public class FirstFragment extends Fragment {

        ......

        public interface OnSelectedListener {
            public abstract void onSelected(String msg);
        }
    }
    ```

1. Implement the Interface in the Activity Class
    ```java
    public class MainActivity extends AppCompatActivity implements FirstFragment.OnSelectedListener {

        ......

        @Override
        public void onSelected(String msg) {
            TextView my_text = (TextView) findViewById(R.id.my_text);
            my_text.setText(msg);
        }
    }
    ```

1. Get the ```OnSelectedListener``` object in Fragment class
    ```java
    public class FirstFragment extends Fragment {

        OnSelectedListener myListener;

        @Override
        public void onAttach(Context context) {
            super.onAttach(context);
            try {
                myListener = (OnSelectedListener) context;
            }catch (ClassCastException e){
                Log.e("FirstFragment", e);
            }
        }

        ......
    }
    ```

1. Call the ```onSelected``` method of ```OnSelectedListener``` object in Fragment Class

    ```java
    public class FirstFragment extends Fragment {

        ......

        @Nullable
        @Override
        public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, Bundle savedInstanceState) {
            View view = inflater.inflate(R.layout.myfragment, container, false);
            Button btn = (Button) view.findViewById(R.id.frag_btn);

            btn.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    myListener.onSelected("I have listened!!!!!");
                }
            });
            return view;
        }

        ......
    }
    ```
#### important things to note
* All subclasses of Fragment must include a public no-argument constructor


#### How to restore back Fragment's state
* to store the Fragment's state you have to override ```onSaveInstanceState(Bundle outState)``` method. In this method you can store data in the *outState* bundle.
* you can get back the data you saved from __savedInstanceState__ , bundle type parameter of the method ```onCreateView(...)```.

#### [some important methods of FragmentTransaction](https://developer.android.com/reference/android/app/FragmentTransaction.html)
1. add
1. remove
1. replace
1. attach
1. detach
1. show
1. hide

#### Some Special Types of Fragment
1. [DialogFragment](https://developer.android.com/reference/android/app/DialogFragment.html)
    * [Dialogs](https://developer.android.com/guide/topics/ui/dialogs.html)
1. [ListFragment](https://developer.android.com/reference/android/app/ListFragment.html)
1. [PreferenceFragment](https://developer.android.com/reference/android/preference/PreferenceFragment.html)
1. [WebViewFragment](https://developer.android.com/reference/android/webkit/WebViewFragment.html)


#### Basic ViewPager
1. Create Fragment Classes
1. Ceate adapter for ViewPager
    * extends any of followings PagerAdapter, FragmentPagerAdapter, FragmentStatePagerAdapter
    * override following methods
        * ```public Fragment getItem(int pos)```
        * ``` public int getCount()```
        * ```public CharSequence getPageTitle(int position)```
1. Add ```ViewPager``` in the Layout file of Activity class
1. Add ```TabLayout``` of Design Support Library inside the ```ViewPager``` and set layout_gravity=Top
1. Or add ```PagerTitleStrip``` of Support Library inside the ```ViewPager``` and set layout_gravity=Top
1. In the activity class find the ViewPager and set adapter to it.
1. Enjoy....


#### Tabs and viewpager
* [Creating Tab with Fragment and ViewPager](https://stackoverflow.com/questions/18413309/how-to-implement-a-viewpager-with-different-fragments-layouts/18413437#18413437)
* [Tabs using __TabLayout__ of Design Support Library](https://github.com/codepath/android_guides/wiki/Google-Play-Style-Tabs-using-TabLayout)
    * [The TabLaout should be inside of ViewPager](https://developer.android.com/reference/android/support/design/widget/TabLayout.html)
* [Tabs using __PagerTitleStrip__](https://developer.android.com/training/implementing-navigation/lateral.html)
* [infinite-viewpager-with-tablayout](https://stackoverflow.com/questions/40292553/infinite-viewpager-with-tablayout)

### Links
* [Replace One Fragment with Another](https://developer.android.com/training/basics/fragments/fragment-ui.html#Replace)
* [Fragments](https://developer.android.com/guide/components/fragments.html)
* [Fragment Tutorial](https://github.com/codepath/android_guides/wiki#fragments)
