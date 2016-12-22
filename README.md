This project is no longer being maintained
==========================================

sticky-headers-recyclerview
===========================

This decorator allows you to easily create section headers for RecyclerViews using a
LinearLayoutManager in either vertical or horizontal orientation.

Credit to [Emil Sjölander](https://github.com/emilsjolander) for creating StickyListHeaders,
a library that many of us relied on for sticky headers in our listviews.

Here is a quick video of it in action (click to see the full video):

[![animated gif demo](http://i.imgur.com/I0ztoPw.gif)](https://www.youtube.com/watch?v=zluBwbf3aew)

[![animated gif demo](http://i.imgur.com/b5pJjtL.gif)](https://www.youtube.com/watch?v=zluBwbf3aew)

Download
--------

Current version: [![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.timehop.stickyheadersrecyclerview/library/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.timehop.stickyheadersrecyclerview/library)

    compile 'com.timehop.stickyheadersrecyclerview:library:[latest.version.number]@aar'


Usage
-----

There are three main classes, `StickyRecyclerHeadersAdapter`, `StickyRecyclerHeadersDecoration`,
and `StickyRecyclerHeadersTouchListener`.

`StickyRecyclerHeadersAdapter` has a very similar interface to the `RecyclerView.Adapter`, and it
is recommended that you make your `RecyclerView.Adapter` implement `StickyRecyclerHeadersAdapter`.

There interface looks like this:

```java
public interface StickyRecyclerHeadersAdapter<VH extends RecyclerView.ViewHolder> {
  public long getHeaderId(int position);

  public VH onCreateHeaderViewHolder(ViewGroup parent);

  public void onBindHeaderViewHolder(VH holder, int position);

  public int getItemCount();
}
```

The second class, `StickyRecyclerHeadersDecoration`, is where most of the magic happens, and does
not require any configuration on your end.  Here's an example from `onCreate()` in an activity:

```java
mRecyclerView = (RecyclerView) findViewById(R.id.recyclerview);
mAdapter = new MyStickyRecyclerHeadersAdapter();
mRecyclerView.setAdapter(mAdapter);
mRecyclerView.setLayoutManager(new LinearLayoutManager(context));
mRecyclerView.addItemDecoration(new StickyRecyclerHeadersDecoration(mAdapter));
```

`StickyRecyclerHeadersTouchListener` allows you to listen for clicks on header views.
Simply create an instance of `StickyRecyclerHeadersTouchListener`, set the `OnHeaderClickListener`,
and add the `StickyRecyclerHeadersTouchListener` as a touch listener to your `RecyclerView`.

```java
StickyRecyclerHeadersTouchListener touchListener =
    new StickyRecyclerHeadersTouchListener(recyclerView, headersDecor);
touchListener.setOnHeaderClickListener(
    new StickyRecyclerHeadersTouchListener.OnHeaderClickListener() {
asdfs      public void onHeaderClick(View header, int position, long headerId) {
        Toast.makeText(MainActivity.this, "Header position: " + position + ", id: " + headerId,
            Toast.LENGTH_SHORT).show();
      }
    });
mRecyclerView.addOnItemTouchListener(touchListener);
```

The StickyHeaders aren't aware of your adapter so if you must notify them when your data set changes.

```java
    mAdapter.registerAdapterDataObserver(new RecyclerView.AdapterDataObserver() {
      @Override public void onChanged() {
        headersDecor.invalidateHeaders();
      }
    });
```

If the Recyclerview's layout manager implements getExtraLayoutSpace (to preload more content then is
visible for performance reasons), you must implement ItemVisibilityAdapter and pass an instance as a
second argument to StickyRecyclerHeadersDecoration's constructor.
```java
    @Override
    public boolean isPositionVisible(final int position) {
        return layoutManager.findFirstVisibleItemPosition() <= position
            && layoutManager.findLastVisibleItemPosition() >= position;
    }
```


Item animators don't play nicely with RecyclerView decorations, so your mileage with that may vary.

Compatibility
-------------

API 11+

Known Issues