* Use composition with components then inheritance.

* Redux

    ## Step 1

    ```
    -> configureStore FROM @reduxjs/toolkit
        const store = configureStore();
    -> Provider from react-redux 
        <Provider store={store}>
        <App />
        </Provider>
    ```

    ## Step 2

        ```
            createSlice from  reduxjs/toolkit

            const someSlice = createSlice({
                name: 'sliceName',
                initialState : { value: boolean},
                reducers: {
                    reducer1: (state) => change state in this method 
                    reducer2: (state) => change value here
                }
            })

            // it creates actions by default

            export  const { reducer1, reducer 2} = someSlice.actions;
            export default someSlice.reducer;
            ```        
   ## Step 3

        // register reducer
        ```
        import someReducer from someSlice
        export const store = createSlice( {
            reducer: {
                customName: someReducer
            }
        })
        ```

    ## step 4
    // get the actions from reducer with custom hook
        ```

        import { useSelector, useDispatch } from 'react-redux';
         const someConst = useSelector( state => state.reducer1.value);
         ```
    // for triggering  actions 
    ```
        const dispatch = useDispatch()

        onclick = { () => dispatch(reducer1())}

    ```
 ## file organization
 * function based organization for smaller or medium size projects.
 * Feature based organization for larger projects