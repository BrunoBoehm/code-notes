GraphQL and React

Side to the React app, the graphQL client on the browser sends the graphql query to the express/graphQL server, that then fetches the datastore

graphQL clients (interface between React and the backend graphql server): 
- Lock : simple with some caching
- Apollo client (from meteor JS) : quite simple and complete, also includes the backend to replace express
- Relay : used by Facebook, really complicated, overkill for startup

graphQL backend
- Apollo server : different way to write graphql, might have API changes, uses Types and Resolvers for schemas
- Express graphQL : reference implementation of GQL, might not evolve (stable)

# Front End
Web pack takes all client code and makes it ready for the browser during development.
The documentation panel in Graphiql gives info about the schema.

In the client folder, index.js file let’s set up Apollo client and wrap our application in the Apollo Provider.
The Provider is connected to the store.
The Apollo Store connects to the server and stores the info that comes back from the server.

```js
// Inside of /client/index.js
import React from 'react';
import ReactDOM from 'react-dom';

const Root = () => {
    return <div>Lyrical</div>
};

ReactDOM.render(
    <Root />,
    document.querySelector('#root')
);
```
We import the Apollo client (that interacts with the backend) and the Apollo Provider for react.
We refactor ou root component to be inside the Provider.
We pass in the client to the Provider as a prop, just like in Redux when using a store.

```js
// Inside of /client/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import ApolloClient from 'apollo-client';
import { ApolloProvider } from 'react-apollo';

const client = new ApolloClient({});

const Root = () => {
    return (
        <ApolloProvider client={client}>
            <div>Lyrical</div>
        </ApolloProvider>
    )
};

ReactDOM.render(
    <Root />,
    document.querySelector('#root')
);
```

We can now make graphQL queries in our React app!

## Connected components
Let's create a `/components` folder in our `/client` folder and create our first `SongList.js` component.
Let's create a class based component to be able to benefit from some helper methods (but no state).
```js
import React, { Component } from 'react';

class SongList extends Component {
    render() {
        return (
            <div>
              SongList
            </div>
        );
    }
}

export default SongList;
```

We can now import this component into our app

```js
// Inside of /client/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import ApolloClient from 'apollo-client';
import { ApolloProvider } from 'react-apollo';

import SongList from './components/SongList';

const client = new ApolloClient({});

const Root = () => {
    return (
        <ApolloProvider client={client}>
            <SongList />
        </ApolloProvider>
    )
};

ReactDOM.render(
    <Root />,
    document.querySelector('#root')
);
```

## First query: reading
Let's now add a query!
- identify the data required by the component
- test the query in GraphiQL, then paste it in the component
- bond query and component
- access data and check!

So if we only need the title of the song, we can create a query in GraphiQL like
```js
{
    songs {
        title
    }
}
```
Once we are satisfied with the return data in GraphiQL we can move to our component.

We can now write in our component. It uses template strings from ES6, and use the gql tag helper library. We name it `gql` by convention.
```js
import React, { Component } from 'react';
import gql from 'graphql-tag';

class SongList extends Component {
    render() {
        return (
            <div>
              SongList
            </div>
        );
    }
}

const query = gql`
    {
        songs {
            title
        }
    }
`;


export default SongList;
```
This just defines the query and assigns it to the `const query`, but does not execute it for us (no request).

We then need to bond the query and the component together. We need to import `import { graphql } from 'react-apollo';`. Then we use the same kind of syntax we use when using Redux. The first part returns a function, that we directly invoque with the SongList component as parameter. 
```js
import React, { Component } from 'react';
import gql from 'graphql-tag';
import { graphql } from 'react-apollo';

class SongList extends Component {
    render() {
        return (
            <div>
              SongList
            </div>
        );
    }
}

const query = gql`
    {
        songs {
            title
        }
    }
`;


export default graphql(query)(SongList);
```
This bonding actually executes the query asynchronously when the component is rendered (or re-dendered). 
Actually is will be rendered the first time without data, and then re-rendered when the query is resolved.
When resolved the data is placed in the props object. We can then use `this.props` (let's `console.log(this.props)` it).
We see 2 console log, the first props object has a `data` property (given by GraphQL `graphql(query)()`) with `loading:true` but no payload. The second contains our information!

```js
import React, { Component } from 'react';
import gql from 'graphql-tag';
import { graphql } from 'react-apollo';

class SongList extends Component {
    render() {
        console.log(this.props);

        return (
            <div>
              SongList
            </div>
        );
    }
}

const query = gql`
    {
        songs {
            title
        }
    }
`;


export default graphql(query)(SongList);
```

Let's now put the data to the screen.
Let's create a helper method like `renderSongs()` to iterate through all results of the query
```js
renderSongs() {
    return this.props.data.songs.map(song => {
        return (
            <li>
                { song.title }
            </li>
        );
    });
}
```

And we can call this helper method from inside of our rendered component.
We need to make sure that we only `.map` on data that has already been fetched by graphQL, otherwise it will throw an error becque `this.props.data.songs` is undefined. For this we can use `this.props.data.loading` as a test and it will `return` early.
```js
import React, { Component } from 'react';
import gql from 'graphql-tag';
import { graphql } from 'react-apollo';

class SongList extends Component {
    renderSongs() {
        return this.props.data.songs.map(song => {
            return (
                <li key={ song.id }>
                    { song.title }
                </li>
            );
        });
    }

    render() {
        // console.log(this.props);
        if (this.props.data.loading) { return <div>Loading...</div> }

        return (
            <ul>
              { this.renderSongs() }
            </ul>
        );
    }
}

const query = gql`
    {
        songs {
            id
            title
        }
    }
`;


export default graphql(query)(SongList);
```
Note we added the id to enable the React `key`.

## Creating a song 
We can add react router
Let's import into our root component: `import { Router, Route, hashHistory, IndexRoute } from 'react-router';`

```js
// Inside of /client/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import { Router, Route, hashHistory, IndexRoute } from 'react-router';
import ApolloClient from 'apollo-client';
import { ApolloProvider } from 'react-apollo';

import App from './components/App';
import SongList from './components/SongList';

const client = new ApolloClient({});

const Root = () => {
    return (
        <ApolloProvider client={client}>
            <Router history={hashHistory}>
                <Route path="/" component={App}>
                    <IndexRoute component={SongList}  />
                </Route>
            </Router>
        </ApolloProvider>
    )
};

ReactDOM.render(
    <Root />,
    document.querySelector('#root')
);
```
The Apollo provider is wrapping the route.
We create and `indexRoute` with our `SongList` component.

We create an `App` component 
```js
import React from 'react';

export default ({ children }) => {
    return <div className="container">{ children }</div>
};
```
We destructure the props object, taking the `children` and pass them inside our div: `SongList` will be passed as a child of `App`.

Let's create a new component/page to create a new song (a mutation to change the data in our server).
This component will call the mutation (collocation).

Let's create the `SongCreate.js` class-based component (we need a form with a controlled input : we watch for a change event and update the state, and put the state back into the input itself).
With the onSubmit we will call our mutation.
```js
import React, { Component } from 'react';

class SongCreate extends Component {
    constructor(props) {
        super(props);

        this.state = { title: "" };
        this.onSubmit = this.onSubmit.bind(this);
    }

    onSubmit(event) {
        event.preventDefault();

        // this is where we'll make our GraphQL query
    }

    render() {
        return (
            <div>
                <h3>Create a new song</h3>
                <form onSubmit={this.onSubmit}>
                    <label>Song Title:</label>
                    <input 
                        onChange={e => this.setState( { title: e.target.value } )}
                        value={this.state.title}
                    />
                </form>
            </div>
        );
    };
}

export default SongCreate;
```

We can import this in our root component, and create a new route.
```js
// Inside of /client/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import { Router, Route, hashHistory, IndexRoute } from 'react-router';
import ApolloClient from 'apollo-client';
import { ApolloProvider } from 'react-apollo';

import App from './components/App';
import SongList from './components/SongList';
import SongCreate from './components/SongCreate';

const client = new ApolloClient({});

const Root = () => {
    return (
        <ApolloProvider client={client}>
            <Router history={hashHistory}>
                <Route path="/" component={App}>
                    <IndexRoute component={SongList}  />
                    <Route path="songs/new" component={SongCreate} />
                </Route>
            </Router>
        </ApolloProvider>
    )
};

ReactDOM.render(
    <Root />,
    document.querySelector('#root')
);
```

Let's test the query in graphiQL. We ask to get back id and title.
```js
mutation {
    addSong(title: "Dog Call") {
        id
        title
    }
}
```

We can move this to our component, we can import the `gql` helper to help JSX understand the syntax of GraphQL.
```js
import React, { Component } from 'react';
import { graphql } from 'react-apollo';
import gql from 'graphql-tag';
import { Link, hashHistory } from 'react-router';

class SongCreate extends Component {
    constructor(props) {
        super(props);

        this.state = { title: "" };
        this.onSubmit = this.onSubmit.bind(this);
    }

    onSubmit(event) {
        event.preventDefault();

        // this is where we'll make our GraphQL query
        // console.log(this.props);
        this.props.mutate({
            variables: {
                title: this.state.title
            }
        }).then(() => hashHistory.push('/'))
    }

    render() {
        return (
            <div>
                <Link to="/">Back</Link>
                <h3>Create a new song</h3>
                <form onSubmit={this.onSubmit}>
                    <label>Song Title:</label>
                    <input 
                        onChange={e => this.setState( { title: e.target.value } )}
                        value={this.state.title}
                    />
                </form>
            </div>
        );
    };
}

const mutation = gql`
mutation AddSong($title: String) {
    addSong(title: $title) {
        id
        title
    }
}
`;

export default graphql(mutation)(SongCreate);
```
Since the mutation needs the title but the state can only be used inside of the component instance, we have to use a specific graphQL solution to communicate from the component back to the mutation.

The feature looks like this: we use the mutation like a function with `query variables` (bottom of the GraphiQL interface). We need it when a component has to customize the query (paginate, filter... the query).
We need to name the mutation like `AddSong` (anything we want, not tied to the backend) to make it sort of a function we can call anywhere in our application to invoke the mutation.
```js
mutation {
    addSong(title: "Yeah") {
        id
        title
    }
}

// becomes as a dynamic version
// $title is the name of the parameter, then parameter type. We can have several parameters.
// $ - type - name - $
mutation AddSong($title: String) {
    addSong(title: $title) {
        id
        title
    }
}

// the associated query variable could be
{
    "title": "Sprite Vs Coke"
} 
```

Instead of getting access to `props.data` we got access to `props.mutate`.
We could then use `props.mutate` inside of our component to provide the dynamic parameters we need to pass to our graphQL query.
```js
    onSubmit(event) {
        event.preventDefault();

        this.props.mutate({
            variables: {
                title: this.state.title
            }
        }).then(() => hashHistory.push('/'));
    }
```
We'll see later how can have several mutations tied to a single component.

To redirect the user back to the list of songs after the song has been created, we can chain `.then` to the promise returned by `mutate`.


### Navigation to create the song
Let's add some navigation by using the `Link` tag from react router.

```js
import React, { Component } from 'react';
import gql from 'graphql-tag';
import { graphql } from 'react-apollo';
import { Link } from 'react-router';

class SongList extends Component {
    renderSongs() {
        return this.props.data.songs.map(song => {
            return (
                <li key={ song.id }>
                    { song.title }
                </li>
            );
        });
    }

    render() {
        // console.log(this.props);
        if (this.props.data.loading) { return <div>Loading...</div> }

        return (
            <div>
                <ul>
                { this.renderSongs() }
                </ul>
                <Link 
                    to="/songs/new"
                    className="btn-floating btn-large btn-right"
                >
                    <i className="material-icons">add</i>
                </Link>
            </div>
        );
    }
}

const query = gql`
    {
        songs {
            id
            title
        }
    }
`;


export default graphql(query)(SongList);
```

### Re-fetching queries
When songs have been fetched already by listing them, when creating a song it will not re-fetch : we only re-render the old items without the new one... let's fix that. We need to re-run the query after the mutation has been done. That's why we can improve the `mutate` function using the `refetchQueries` property : `refetchQueries: [{ query: query }]`. We need to put our query into a separate file so that we are able to re-run it.

We create a folder called `client/queries` and call it `fetchSongs.js`. This way we can call the query from anywhere in our app instead of copying it.
```js
import gql from 'graphql-tag';

export default gql`
    {
        songs {
            id
            title
        }
    }
`;
```

Let's refactor our `SongList` component
```js
import React, { Component } from 'react';
import gql from 'graphql-tag';
import { graphql } from 'react-apollo';
import { Link } from 'react-router';

import query from '../queries/fetchSongs';

class SongList extends Component {
    renderSongs() {
        return this.props.data.songs.map(song => {
            return (
                <li key={ song.id }>
                    { song.title }
                </li>
            );
        });
    }

    render() {
        // console.log(this.props);
        if (this.props.data.loading) { return <div>Loading...</div> }

        return (
            <div>
                <ul>
                { this.renderSongs() }
                </ul>
                <Link 
                    to="/songs/new"
                    className="btn-floating btn-large btn-right"
                >
                    <i className="material-icons">add</i>
                </Link>
            </div>
        );
    }
}

export default graphql(query)(SongList);
```

We also need to refactor our `SongCreate` component to integrate an immediate rerun of the "All Songs" query after a new song has been added.
```js
import React, { Component } from 'react';
import { graphql } from 'react-apollo';
import gql from 'graphql-tag';
import { Link, hashHistory } from 'react-router';

import query from '../queries/fetchSongs';

class SongCreate extends Component {
    constructor(props) {
        super(props);

        this.state = { title: "" };
        this.onSubmit = this.onSubmit.bind(this);
    }

    onSubmit(event) {
        event.preventDefault();

        this.props.mutate({
            variables: { title: this.state.title },
            refetchQueries: [{ query }]     // ES6 shortening query: query
        }).then(() => hashHistory.push('/'))
    }

    render() {
        return (
            <div>
                <Link to="/">Back</Link>
                <h3>Create a new song</h3>
                <form onSubmit={this.onSubmit}>
                    <label>Song Title:</label>
                    <input 
                        onChange={e => this.setState( { title: e.target.value } )}
                        value={this.state.title}
                    />
                </form>
            </div>
        );
    };
}

const mutation = gql`
mutation AddSong($title: String) {
    addSong(title: $title) {
        id
        title
    }
}
`;

export default graphql(mutation)(SongCreate);
```

Apollo/GraphQL will not attempt to run the same query twice in a row: when we ask `SongCreate` to rerun the query and then get redirected to the `SongList`page, there will be no additional query.

## Deleting a song
An icon on each song will call a mutation to erase that song from the DB.

In GraphiQL, from the ID of a song we will provide this using query variables. From our backend API the `deleteSong` action can be used.
```js
// query variable
{
    id: "6iudcz789054345"
}

// "named" mutation (to use query variables)
// ID is just another type like String or Integer, available in the graphQL schema
mutation DeleteSong($id: ID) {
    deleteSong(id: $id) {
        id
    }
}
```

The mutation will be called from the SongList component, where we render all songs with our `renderSongs` helper method. We could create an external file to store the mutation and make it "modularized", but as we don't expect to use this mutation anywhere else in the codebase, we can leave it as is.

We already have `export default graphql(query)(SongList);` with our `query` inside. We cannot pass the mutation as a second argument. We need to use multiple instances of the helper. Third party solutions exist but this is the Apollo way for now, might evolve.

For every song we show a delete icon (from the materialize icon set), with a callback function onClick to call the mutation: we access as always `this.props.mutate` and pass our query arguments as a `variables` object.
We also re-run the query for listing the songs, otherwise the song will be deleted but still appear on the UI as long as we don't refresh or switch page. We can use a different approach based on the promise that is returned from the mutation, and chaining `this.props.data.refetch()`: the refetch function will use the query associated to the component (≠ from previous case where the query was not linked to the component).
```js
import React, { Component } from 'react';
import gql from 'graphql-tag';
import { graphql } from 'react-apollo';
import { Link } from 'react-router';

import query from '../queries/fetchSongs';

class SongList extends Component {
    onSongDelete(id) {
        this.props.mutate({ variables: { id } }) // for {id: id}
            .then( () => this.props.data.refetch() )
    }

    renderSongs() {
        return this.props.data.songs.map( {id, title} => {
            return (
                <li key={ id }>
                    { title }
                    <i
                        className="material-icons"
                        onClick={ () => this.onSongDelete(id) }
                    >delete</i>
                </li>
            );
        });
    } 

    render() {
        // console.log(this.props);
        if (this.props.data.loading) { return <div>Loading...</div> }

        return (
            <div>
                <ul>
                { this.renderSongs() }
                </ul>
                <Link 
                    to="/songs/new"
                    className="btn-floating btn-large btn-right"
                >
                    <i className="material-icons">add</i>
                </Link>
            </div>
        );
    }
}

const mutation = gql`
    mutation DeleteSong($id: ID) {
        deleteSong(id: $id) {
            id
        }
    }
`;

export default graphql(mutation)(
    graphql(query)(SongList)
);
```

Note how we refactored using destructuring
```js
// before
    renderSongs() {
        return this.props.data.songs.map(song => {
            return (
                <li key={ song.id }>
                    { song.title }
                    <i
                        className="material-icons"
                        onClick={ () => this.onSongDelete(song.id) }
                    >delete</i>
                </li>
            );
        });
    }

// after
    renderSongs() {
        return this.props.data.songs.map( {id, title} => {
            return (
                <li key={ id }>
                    { title }
                    <i
                        className="material-icons"
                        onClick={ () => this.onSongDelete(id) }
                    >delete</i>
                </li>
            );
        });
    } 
```

### Editing the CSS
We want the icon to move to the right and have a pointer for the icon.
Inside of the `client/style` folder we can add
```css
.collection-item {
    display: flex;
    justify-content: space-between;
}

.material-icons {
    cursor: pointer;
}
```

We can import the CSS directly into our root component with `import './style/style.css';`

## Working on the Song Detail page
It lists all lyrics of the song, as well as a form to create new lyrics.

Let's create a new file `components/SongDetail`
```js
import React, { Component } from 'react';

class SongDetail extends Component {
    render() {
        <div>
            <h3>Song Detail</h3>
        </div>
    }
}

export default SongDetail;
```

We can now add a new route in our root `index.js` file.
```js
import React from 'react';
import ReactDOM from 'react-dom';
import { Router, Route, hashHistory, IndexRoute } from 'react-router';
import ApolloClient from 'apollo-client';
import { ApolloProvider } from 'react-apollo';

import App from './components/App';
import SongList from './components/SongList';
import SongCreate from './components/SongCreate';
import SongDetail from './components/SongDetail';

const client = new ApolloClient({});

const Root = () => {
    return (
        <ApolloProvider client={client}>
            <Router history={hashHistory}>
                <Route path="/" component={App}>
                    <IndexRoute component={SongList}  />
                    <Route path="songs/new" component={SongCreate} />
                    <Route path="songs/:id" component={SongDetail} />
                </Route>
            </Router>
        </ApolloProvider>
    )
};

ReactDOM.render(
    <Root />,
    document.querySelector('#root')
);
```

Now the boilerplate works, but we need to fetch the right song...
Our API has a `song(id: ID!): SongType`. Inside of GraphiQL we can test the query
```js
// query variable
{
    "id": "456789ghjks" 
}

// "named" query: no enforced requirement for the name
query SongQuery($id: ID!) { // ID needs to be here
    song(id: $id){          // query from our API
        id
        title
    }
}
```

We can now move to our SongDetail component, and we can create a new query file in `queries/fetchSong.js`
```js
import gql from 'graphql-tag';

export default gql`
    query SongQuery($id: ID!) { 
        song(id: $id){          
            id
            title
        }
    }
`
```

In our SongDetail component, we need to import `graphql` with `import { graphql } from 'react-apollo';` to sandwitch together our query and the component.
We also import our `fetchSong` query.
We need to get from our route the specific ID of the song: we can access it from the react-router param from `this.props.params.id`.
To push down the ID to the fetchSong (that gets executed automatically, as opposed to mutation that are manual / onClick), we can use the options parameter. The graphql helper is 100% aware of the props.
```js
import React, { Component } from 'react';
import { graphql } from 'react-apollo';
import fetchSong from '../queries/fetchSong';

class SongDetail extends Component {
    render() {
        // console.log(this.props);
        <div>
            <h3>Song Detail</h3>
        </div>
    }
}

export default graphql(fetchSong, {
    options: (props) => { return { variables: {id: this.props.params.id} } } // same props as the inside the component class
})(SongDetail);
```

# Back End
MongoLab (MongoDB)

We need an express/graphql: `npm install --save express express-graphql graphql lodash`.

We need a server.js file
```js
const express = require('express');

const app = express();

app.listen(4000, () => {
    console.log("listening on port 4000")
})
```

As a server, Express processed an incoming `req`uest and sends back a `res`ponse. Inside of the server, it will examine the request to see if it is graphql-related, and if so it will hand it over to graphql, that will process and send back a request, that Express will pass on to the user. The rest of Express can still work (authentication, templating...).

Let's require `express-graphql` and make sure any request that includes `/graphql` will be sent to GraphQL dev interface (for the moment).
```js
const express = require('express');
const expressGraphQL = require('express-graphql');

const app = express();

// .use middleware
app.use('/graphql'; expressGraphQL({
    graphiql: true          // option object
}))

app.listen(4000, () => {
    console.log("listening on port 4000")
})
```

We can run our server with `node server.js` in our terminal. Our schema is missing! We need to make GraphQL aware of the structure of the data.
Let's create a folder called `/schema` and a `schema.js` file.
```js
const graphql = require('graphql');

const {
    GraphQLObjectType,       // destructuring
    GraphQLString,
    GraphQLInt
} = graphql;

// defining the concept of "User"
const UserType = new GraphQLObjectType({
    name: 'User',
    fields: {               // all properties with data types defined by built-in type object
        id: { type: GraphQLString },
        firstName: { type: GraphQLString },
        age: { type: GraphQLInt }  
    }
});
```

We need a root query to tell GraphQL which entry point into the data it needs to use based on the query.
```js
const graphql = require('graphql');

const {
    GraphQLObjectType,       // destructuring
    GraphQLString,
    GraphQLInt
} = graphql;

// defining the concept of "User"
const UserType = new GraphQLObjectType({
    name: 'User',
    fields: {               // all properties with data types defined by built-in type object
        id: { type: GraphQLString },
        firstName: { type: GraphQLString },
        age: { type: GraphQLInt }  
    }
});

const RootQuery = new GraphQLObjectType({
    name: 'RootQueryType',
    fields: {
        user: {
            type: UserType,
            args: { id: { type: GraphQLString } },
            resolve(parentValue, args) {        // tells how to get the data. parentValue has no use here, arguments are the ones that are important
                ...
            }
        }
    }
})
```

Let's hard-code a few users in a const.
```js
const graphql = require('graphql');
const _ = require('lodash');

const {
    GraphQLObjectType, 
    GraphQLString,
    GraphQLInt,
    GraphQLSchema       // add this one
} = graphql;

const users = [             // adding dummy data
    { id: '23', firstName: 'Bill', age: 20 },
    { id: '24', firstName: 'Boule', age: 22 },
]

const UserType = new GraphQLObjectType({
    name: 'User',
    fields: {              
        id: { type: GraphQLString },
        firstName: { type: GraphQLString },
        age: { type: GraphQLInt }  
    }
});

const RootQuery = new GraphQLObjectType({
    name: 'RootQueryType',
    fields: {
        user: {
            type: UserType,
            args: { id: { type: GraphQLString } },
            resolve(parentValue, args) {       
                return _.find(users, { id: args.id });      // our resolve function query that return raw javascript and convert it BTS
            }
        }
    }
})

module.exports = new GraphQLSchema({
    query: RootQuery
});
```

We can now import this from our server file.
```js
const express = require('express');
const expressGraphQL = require('express-graphql');
const schema = require('./schema/schema');      // import

const app = express();

app.use('/graphql'; expressGraphQL({
    schema,             // schema: schema (ES6)
    graphiql: true      
}))

app.listen(4000, () => {
    console.log("listening on port 4000")
})
```
Let's restart the server, watch for typos and capitalization. GraphiQL interface should be available at `localhost:4000/graphql`.
We can now make our first query!
```js
{
    user(id: "23") {
        id, 
        firstName,
        age
    }
}
```

We could have several data stores (servers, API, database...) instead of a single database, just like Facebook.

## Talking to an API
Let's make a second server like JSON server to simulate an API.
`npm install --save json-server`

New file `db.json` with JSON data inside it, using double quotes `"`.
```json
{
    "users": [
        { "id": "23", "firstName": "Bill", "age": 20, "company_id": "1" },
        { "id": "24", "firstName": "Boule", "age": 22, "company_id": "1" },
        { "id": "24", "firstName": "Papa", "age": 55, "company_id": "2" }
    ], 
    "companies": [
        { "id": "1", "name": "Apple", "description": "iphone"},
        { "id": "2", "name": "Google", "description": "search"}
    ]
}
```

The resolve function can return a synchronous response, but also an asynchronous promise. We'll nearly always use promises. We can use fetch() or axios: `npm install --save axios`. We don't need lodash anymore.

```js
const graphql = require('graphql');
const axios = require('axios');

const {
    GraphQLObjectType, 
    GraphQLString,
    GraphQLInt,
    GraphQLSchema    
} = graphql;

const UserType = new GraphQLObjectType({
    name: 'User',
    fields: {              
        id: { type: GraphQLString },
        firstName: { type: GraphQLString },
        age: { type: GraphQLInt }  
    }
});

const RootQuery = new GraphQLObjectType({
    name: 'RootQueryType',
    fields: {
        user: {
            type: UserType,
            args: { id: { type: GraphQLString } },
            resolve(parentValue, args) {       
                return axios.get(`localhost:3000/users/${args.id}`)         // ES6 template string
                    .then(resp => resp.data);               // Nested: { data: {firstName: 'Bill' } }, GrapgQL could not interpret the resp alone
            }
        }
    }
})

module.exports = new GraphQLSchema({
    query: RootQuery
});
```

### Note : Improving the server with Nodemon
Nodemon will reboot at code change: `npm install --save nodemon`.
in our package.json we create a `dev` script:
```js
"dev": "nodemon server.js"
```

Let's run it: `npm run dev`.

## Company Type
Let's improve with a Company related to a User (company_id).

In the schema let's create the company above the user!
```js
...
const CompanyType = new GraphQLObjectType({
    name: "Company";
    fields: {
        id: { type: GraphQLString }, 
        name: { type: GraphQLString },
        description: { type: GraphQLString }
    }
});

// define users below (in that order)
const UserType = new GraphQLObjectType({
    name: 'User',
    fields: {              
        id: { type: GraphQLString },
        firstName: { type: GraphQLString },
        age: { type: GraphQLInt },
        company: {                  // and not companyId, we need a resolve function to make the link!
            type: CompanyType,
            resolve(parentValue, args){   // parentValue will be the company
                // console.log(parentValue, args);
                return axios.get(`http://localhost:3000/companies/${parentValue.companyId}`)
                    .then(res => res.data);
            }
        }
    }
});
```

We can now make requests like
```js
{
    user(id: "23") {
        id, 
        firstName,
        company {
            id,
            name,
            description
        }
    }
}
```

## Querying a company
For the moment we can only go from a user to a company, let's enable to go directly to a company, inside of the root query.
```js
const RootQuery = new GraphQLObjectType({
    name: 'RootQueryType',
    fields: {
        user: {
            type: UserType,
            args: { id: { type: GraphQLString } },
            resolve(parentValue, args) {       
                return axios.get(`localhost:3000/users/${args.id}`)       
                    .then(resp => resp.data);          
            }
        },
        company: {              // let's add the company root query type
            type: CompanyType,
            args: { id: type: GraphQLString },
            resolve(parentValue, args) {
                return axios.get(`localhost:3000/companies/${args.id}`)
                    .then(resp => resp.data);
            }
        }
    }
})
```

## Linking Companies and Users
From a company root query we want to be able to know which users belong to that specific company: our API data point for `/companies/1/users`.
Let's improve the `CompanyType`.

```js
const {
    GraphQLObjectType, 
    GraphQLString,
    GraphQLInt,
    GraphQLSchema,
    GraphQLList  
} = graphql;
...
const CompanyType = new GraphQLObjectType({
    name: "Company";
    fields: {
        id: { type: GraphQLString }, 
        name: { type: GraphQLString },
        description: { type: GraphQLString },
        users: {
            type: new GraphQLList(UserType),   // will return a list of users. We need to import it by destructuring too.
            resolve(parentValue, args){
                return axios.get(`localhost:3000/companies/${parentValue.id}/users`)
                    .then(res => res.data);
            }
        }
    }
});
...
```

We have a *circular reference* error coming from `new GraphQLList(UserType)`!!! It has not been defined! We can use an arrow function to the rescue (its just a closure scope javascript issue), like:
```js
const CompanyType = new GraphQLObjectType({
    name: "Company";
    fields: () => ({
        id: { type: GraphQLString }, 
        name: { type: GraphQLString },
        description: { type: GraphQLString },
        users: {
            type: new GraphQLList(UserType),   // will return a list of users. We need to import it by destructuring too.
            resolve(parentValue, args){
                return axios.get(`localhost:3000/companies/${parentValue.id}/users`)
                    .then(res => res.data);
            }
        }
    }) // end of closure
});
```
### Note: naming queries
we can write and identify a query with a name:
```js
{
    user(id: "23") {
        id, 
        firstName,
        company {
            id,
            name,
            description
        }
    }
}

// becomes...

query findUserWithCompany {
    user(id: "23") {
        id, 
        firstName,
        company {
            id,
            name,
            description
        }
    }
}
```

Or to make duplicate queries
```js
{
    firstUser: user(id: "23") {
        id, 
        firstName,
        company {
            id,
            name,
            description
        }
    }

    secondUser: user(id: "24") {            // renaming the second one is optional
        id, 
        firstName,
        company {
            id,
            name,
            description
        }
    }
}
```

We can also use query fragments to DRY this up.


```js
{
    user(id: "23") {
        ...userDetails
    }
}

fragment userDetails on User {
    id, 
    firstName,
    company {
        id,
        name,
        description
    }
}
```

## Mutations
### Add mutation
Let's create our mutations, starting with a root mutation. The main difference will be what happens inside our mutations.
```js
const {
    GraphQLObjectType, 
    GraphQLString,
    GraphQLInt,
    GraphQLSchema,
    GraphQLList,
    GraphQLNonNull  
} = graphql;

...

const mutation = new GraphQLObjectType({
    name: "Mutation",
    fields: {
        addUser: {
            type: UserType,     // type that comes back
            args: {             // what's needed to perform the mutation
                firstName: { type: new GraphQLNonNull(GraphlQLString) },    // wrapping it with a helper (add it to imports)
                age: { type: new GraphQLNonNull(GraphlQLInt) },
                companyId: { type: GraphlQLString }
            },     
            resolve(parentValue, { firstName, age } ){             // destructure the args
                return axios.post('http://localhost:3000/users', { firstName, age } )
                    .then(res => res.data);
            }
        }
    }
})

module.exports = new GraphQLSchema({
    query: RootQuery,
    mutation            // mutation: mutation
})
```

Let's write our mutation query, we need for properties coming back (from the resolve function)
```js
mutation {
    addUser(firstName: "Stephen", age: 26) {
        id
        firstName
        age
    }
}
```

### Delete Mutation
Let's delete stuff
```js
const mutation = new GraphQLObjectType({
    name: "Mutation",
    fields: {
        addUser: {
            type: UserType,     // type that comes back
            args: {             // what's needed to perform the mutation
                firstName: { type: new GraphQLNonNull(GraphlQLString) },    // wrapping it with a helper (add it to imports)
                age: { type: new GraphQLNonNull(GraphlQLInt) },
                companyId: { type: GraphlQLString }
            },     
            resolve(parentValue, { firstName, age } ){             // destructure the args
                return axios.post('http://localhost:3000/users', { firstName, age } )
                    .then(res => res.data);
            }
        },
        deleteUser: {
            type: UserType,
            args: {
                id: { type: new GraphQLNonNull(GraphQLString) }
            },
            resolte(parentValue, {id}) {   // ES6 Destructuring
                return axios.delete(`http://localhost:3000/users/${id}`)
                    .then(res => res.data);
            }
        }
    }
})
```

We can now call the mutation from GraphiQL, we don't get the ID of the user that was deleted (JSON server).
```js
mutation {
    deleteUser(id: "23") {
        id
    }
}
```
We don't have the ability to say to GraphQL that we don't need anything back.

### Edit Mutation
This last mutation
```js
const mutation = new GraphQLObjectType({
    name: "Mutation",
    fields: {
        addUser: {
            type: UserType,   
            args: {           
                firstName: { type: new GraphQLNonNull(GraphlQLString) },    
                age: { type: new GraphQLNonNull(GraphlQLInt) },
                companyId: { type: GraphlQLString }
            },     
            resolve(parentValue, { firstName, age } ){           
                return axios.post('http://localhost:3000/users', { firstName, age } )
                    .then(res => res.data);
            }
        },
        deleteUser: {
            type: UserType,
            args: {
                id: { type: new GraphQLNonNull(GraphQLString) }
            },
            resolte(parentValue, {id}) {   // ES6 Destructuring
                return axios.delete(`http://localhost:3000/users/${id}`)
                    .then(res => res.data);
            }
        },
        editUser: {
            type: UserType,         // what comes back is a User
            args: {
                id: { type: new GraphQLNonNull(GraphlQLString) },       // compulsory
                age: { type: GraphlQLInt },                             // optional in edit
                companyId: { type: GraphlQLString }                     // optional in edit
            },
            resolve(parentValue, args ){         
                return axios.patch(`http://localhost:3000/users/${args.id}`, args)
                    .then(res => res.data);
            }
        }
    }
})
```

There is a difference between a PUT (completely replace) and PATCH (update) request.
We can send our mutation query
```js
mutation {
    editUser(id: "2", firstName: "Samantha", age: 50, companyId: "3") {
        id
        firstName
        age
        company {
            name
        }
    }
}
```

Facebook has their schema in one single file.

# Connecting front end to backend
