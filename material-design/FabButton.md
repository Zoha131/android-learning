### Animating the Floating Action Button on Scroll
* You have to use CoordinatorLayout
* You have to use recyclerview
* You have to customize Floating Action Button behavior
    ```java
    package io.github.zoha131.designsupportlearning;

    import android.content.Context;
    import android.support.design.widget.CoordinatorLayout;
    import android.support.design.widget.FloatingActionButton;
    import android.support.v4.view.ViewCompat;
    import android.util.AttributeSet;
    import android.util.Log;
    import android.view.View;

    /**
     * Created by abirh on 09-Feb-18.
     */

    public class MyFloatButtnBehavior extends FloatingActionButton.Behavior {

        public MyFloatButtnBehavior(Context context, AttributeSet attrs) {
            super();
        }

        @Override
        public boolean onStartNestedScroll(final CoordinatorLayout coordinatorLayout, final FloatingActionButton child,
                                           final View directTargetChild, final View target, final int nestedScrollAxes) {
            // Ensure we react to vertical scrolling
            return nestedScrollAxes == ViewCompat.SCROLL_AXIS_VERTICAL
                    || super.onStartNestedScroll(coordinatorLayout, child, directTargetChild, target, nestedScrollAxes);
        }

        @Override
        public void onNestedScroll(final CoordinatorLayout coordinatorLayout, final FloatingActionButton child,
                                   final View target, final int dxConsumed, final int dyConsumed,
                                   final int dxUnconsumed, final int dyUnconsumed) {
            super.onNestedScroll(coordinatorLayout, child, target, dxConsumed, dyConsumed, dxUnconsumed, dyUnconsumed);
            if (dyConsumed > 0 && child.getVisibility() == View.VISIBLE) {
                // User scrolled down and the FAB is currently visible -> hide the FAB
                child.hide(new FloatingActionButton.OnVisibilityChangedListener() {
                    @Override
                    public void onHidden(FloatingActionButton fab) {
                        super.onHidden(fab);
                        fab.setVisibility(View.INVISIBLE);
                    }
                });
            } else if (dyConsumed < 0 && child.getVisibility() != View.VISIBLE) {
                // User scrolled up and the FAB is currently not visible -> show the FAB
                child.show();
            }
        }
    }
    ```
    Remember: [floating-action-button-not-visible-on-scrolling-after-updating-google-support](https://stackoverflow.com/questions/41153619/floating-action-button-not-visible-on-scrolling-after-updating-google-support)

* Add this behavior in FAB's xml layout
    ```xml
    <android.support.design.widget.FloatingActionButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/myFloat"
            android:layout_margin="16dp"
            app:layout_anchor="@id/recycler_view"
            app:layout_anchorGravity="bottom|end"
            android:src="@drawable/ic_group_work_black_24dp"
            app:layout_behavior="io.github.zoha131.designsupportlearning.MyFloatButtnBehavior"
            android:layout_gravity="bottom|end"/>
    ```

* Enjoy :smile:
