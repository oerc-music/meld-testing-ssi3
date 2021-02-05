

# Proposals for adding to meld-clients-core

## createStoreWithMiddleware

See:

- `hello-meld-2/App.js`

Looking at some new dependency problems, I’m noticing this:

    const createStoreWithMiddleware = applyMiddleware(thunk, ReduxPromise)(createStore);

If this is defined in the target app, it forces import of a whole load of redux modules that aren’t used anywhere else.  If this function is defined and exported by meld-clients-core, in the case of hello-meld-2, and maybe other apps, we end up with 2 separate copies of the redux modules in the application tree:  one for meld-clients-core (buried in node_modules), and another for the application itself.  This seems wasteful, as well as creating unnecessary dependencies.

A similar refactoring (related to graph traversal) was discussed with DavidL and KP yesterday, and was tentatively earmarked for a MELD 2.1 release.

While doing this, maybe choose a more evocative name (from an application viewpoint)?  Something like `createReduxStore`, maybe?


## graphComponentDidUpdate (graph building traversal)

Some refactored code code in `hello-meld-3/App.js` uses this new function:

    graphComponentDidUpdate(props, prevProps, prevState) {
        var prevPool = prevProps.traversalPool;
        var thisPool = props.traversalPool;
        var updated  = false;
        if (prevPool.running === 1 && thisPool.running ===0){
            // check our traversal objectives if the graph has updated
            props.checkTraversalObjectives(
                props.graph.graph, props.graph.objectives);
            updated = true;
        } else if ( Object.keys(thisPool.pool).length && thisPool.running < MAX_TRAVERSERS) {
            // Initiate next traverser in pool...
            var uri = Object.keys(thisPool.pool)[0];
            props.traverse(uri, thisPool.pool[uri]);
            if (prevProps.graph.outcomesHash !== props.graph.outcomesHash) {
                updated = true;
            }
        } else if ( props.traversalPool.running===0 ) {
            if(prevProps.graph.outcomesHash !== props.graph.outcomesHash) {
                updated = true;
            }
        }
        return updated;
    }

This appears to be quite independent of any application logic, and quite dependent on internal behaviour of the graph traverser.  Therefore, I propose it's addition to meld-clients-core, and discourage/deprecate direct application access to property fields use by the traversal logic (such as `props.traversalPool`).

Also consider renaming?


## index.js rendering <App ...>

In order to support component testing I’m moving my “Hello MELD” apps to use a standard top-level index.js:

    import React  from 'react';
    import ReactDOM from 'react-dom';
    import App from './App';
    ReactDOM.render(
      <React.StrictMode>
        <App />
      </React.StrictMode>,
      document.getElementById('root')
    );

This means that I can test all the UI/App logic without having to fire up a server, hooking instead into a component-level testing framework.

