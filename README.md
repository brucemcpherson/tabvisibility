

# TAB VISIBILITY

When a browser tab goes in and out of view, it's possible you want to do something differently. If you're writing Apps Script add-ons this is especially true. Tab visibility is a class to allow the detection and testing of visibility across multiple browsers.

## usage

````
const tabVisibility = new TabVisibility()
````

## methods

It's a very simple class, with only these methods

| method | returns | purpose |
| --- | --- | --- |
| isVisible | Boolean | whether the tab is currently visible |
| onVisible(action: function) | - | What to do when the tab becomes visible |
| onHidden(action: function) | - | What to do when a tab becomes hidden |

## example

The background of this example is that Apps Script add-on wants to stop polling a spreadsheet for updates when the spreadsheet is not in view. The mechanics of this are quite unimportant, but TabVisibility is used to provoke a change of polling state in the the Vuex store which is behind the add-on. The entire main app simply looks like this, with a reference to a function to handle tab visibility

````
window.onload =  () => {

   
  // initialize everything and register all the components
  Store.init().registerAll()
  
  // render vue
  new Vue({
    el: '#app',
    vuetify: new Vuetify(),
    store: Store.vxStore
  })
  
  // start polling
  Store.load()
 

  /**
   * there's some action that could be taken when the tab becomes visible
   */
  Store.handleVisibility(new TabVisibility())

}
````
And the handler 
````
  ns.handleVisibility = (tabVisibility) => {
    // if the tab goes out of focus, we want to pause everything
    tabVisibility
      .onHidden(()=>ns.vxStore.dispatch('pause', true))
      .onVisible(()=>ns.vxStore.dispatch('pause', false))
  }
````

And there's nothing really more to say.

There's a write up here https://ramblings.mcpher.com/vuejs-apps-script-add-ons/tabvisibility/

