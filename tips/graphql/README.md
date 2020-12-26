# GRAPHQL

## Server

https://docs.google.com/presentation/d/1IrGA4PtUEZPVDTBg5_WCMmUapElbFBgLwfSBAp8ft1g/edit#slide=id.g64f110e442_0_190

https://github.com/FrontendMasters/fullstack-graphql

### First server example

```js
const gql = require('graphql-tag');
const { AppolloServer } = require('appollo-server');

const typeDefs = gpl`
    type User {
        email: String!
        avatar: String
        friends: [User]!
    }

    type Query {
        me: User!
    }
`;

const resolvers = {
  Query: {
    me() {
      return {
        email: 'huhu@haha.com',
        avatar: 'hhhhh',
        friends: [],
      };
    },
  },
};

const server = new AppolloServer({
  typeDefs,
  resolvers,
});

server.listen(4000).then(() => console.log('on port 4000'));
```

### Example 2

```js
// SCHEMA
const { gql } = require('apollo-server');

const typeDefs = gql`
  enum PetType {
    CAT
    DOG
  }
  type User {
    id: ID!
    username: String!
    pets: [Pet]!
  }
  type Pet {
    id: ID!
    type: PetType!
    name: String!
    owner: User!
    img: String!
    createdAt: Int!
  }
  input NewPetInput {
    name: String!
    type: PetType!
  }
  input PetsInput { //Input type
    type: PetType
  }
  type Query {
    user: User!
    pets(input: PetsInput): [Pet]!
    pet(id: ID!): Pet!
  }
  type Mutation {
    addPet(input: NewPetInput!): Pet!
  }
`;

module.exports = typeDefs;

// RESOLVERS
module.exports = {
  Query: {
    pets(_, { input }, { models }) {
      // 2nd arg is called Argument and 3rd is called Context
      return models.Pet.findMany(input || {});
    },
    pet(_, { id }, { models }) {
      return models.Pet.findOne({ id });
    },
    user(_, __, { models }) {
      return models.User.findOne();
    },
  },
  Mutation: {
    addPet(_, { input }, { models, user }) {
      const pet = models.Pet.create({ ...input, user: user.id });
      return pet;
    },
  },
  Pet: {
    owner(pet, _, { models }) {
      return models.User.findOne({ id: pet.user });
    },
    img(pet) {
      return pet.type === 'DOG'
        ? 'https://placedog.net/300/300'
        : 'http://placekitten.com/300/300';
    },
  },
  User: {
    pets(user, _, { models }) {
      return models.Pet.findMany({ user: user.id });
    },
  },
};

// SERVER
const { ApolloServer } = require('apollo-server');
const typeDefs = require('./schema');
const resolvers = require('./resolvers');
const { models, db } = require('./db');

const server = new ApolloServer({
  typeDefs,
  resolvers,
  context() {
    const user = db.get('user').value();
    return { models, db, user };
  },
});

server.listen().then(({ url }) => {
  console.log(`🚀 Server ready at ${url}`);
});
```

## Client

### Operation names

```graphql
query AllCharacters {
  characters {
    results {
      name
    }
  }
}
```

Here `AllCharacters` is the name of the query and it will get cached by GraphQL

### Variable and Argument

call AllCharacters with `{page: 2, filter: {name: 'Rick'}}`

```graphql
query AllCharacters($page: Int, $filter: FilterCharacter) {
  characters(page: $page, filter: $filter) {
    results {
      name
    }
  }
}
```

### Alias

Here `fullName` will alias name and can be accessed like `characters.results[0].fullName`

```graphql
query AllCharacters {
  characters {
    results {
      fullName: name
    }
  }
}
```

### Example

#### index.js

```js
import React from 'react';
import ReactDOM from 'react-dom';
import { BrowserRouter } from 'react-router-dom';
import { ApolloProvider } from '@apollo/react-hooks';
import App from './components/App';
import client from './client';
import './index.css';

const Root = () => (
  <BrowserRouter>
    <ApolloProvider client={client}>
      <App />
    </ApolloProvider>
  </BrowserRouter>
);

ReactDOM.render(<Root />, document.getElementById('app'));

if (module.hot) {
  module.hot.accept();
}
```

#### client.js

```js
import { ApolloClient } from 'apollo-client';
import { InMemoryCache } from 'apollo-cache-inmemory';
import { HttpLink } from 'apollo-link-http';
import gql from 'graphql-tag';

/**
 * Create a new apollo client and export as default
 */

// Add a local state to our client
// Here we extend a type from server
const typeDefs = gpl`
    extend type User {
        age: Int
    }

    extend type Pet {
        vaccinated: Boolean!
    }
 `;

const resolvers = {
  User: {
    age() {
      return 35;
    },
  },

  Pet: {
    vaccinated() {
      return true;
    },
  },
};

// link to server
const http = new HttpLink({ uri: 'http://locahost:4000/' });
const cache = new InMemoryCache();

const client = new ApolloClient({ http, cache, resolvers, typeDefs });

// test if the query works with https://rickandmortyapi.com/graphql
const query = gpl`
    query AllCharacters {
        characters {
            results {
                id
                name
            }
        }
    }
`;

client.query({ query }).then((result) => console.log(result));
// End test

export default client;
```

#### Pages/Pets.js

```js
import React, { useState } from 'react';
import gql from 'graphql-tag';
import { useQuery, useMutation } from '@apollo/react-hooks';
import PetsList from '../components/PetsList';
import NewPetModal from '../components/NewPetModal';
import Loader from '../components/Loader';

// FRAGMENTS
const PETS_FIELDS = gpl`
    fragment PetsFields on Pet {
        name
        id
        img
        type
        vaccinated @client
        owner {
            id
            age @client
        }
    }
`;

// We use the fragment here
const ALL_PETS = gpl`
    query AllPets {
        pets {
            ...PetsFields
        }
    }
    ${PET_FIELDS}
`;

// ^ '@client' directive tells graphQL to uses the client resolver because 'age' was created in the client

const NEW_PET = gpl`
    mutation CreateAPet($newPet: NewPetInput!) {
        addPet(input: $newPet) {
            ...PetsFields
        }
    }
    ${PET_FIELDS}
`;

// Nice to have mutation returning the same object as queries ^

export default function Pets() {
  const [modal, setModal] = useState(false);
  const { data, loading, error } = useQuery(ALL_PETS);

  // 'update' function from useMutation allow to sync Apollo cache to add the new pet to the list of pets. A little tricky but works!!
  const [createPet, newPet] = useMutation(NEW_PET, {
    update(cache, { data: { addPet } }) {
      // addPet is the name of mutation
      const { pets } = cache.readQuery({ query: ALL_PETS });
      cache.writeQuery({
        query: ALL_PETS,
        data: { pets: [addPet, ...pets] },
      });
    },
  }); // newPet is { data, loading, error }

  // `optimisticResponse` allow to do optimistic UI: if we know what will be the response from server we can set update apollo cache with the data which will make the UI seems super fast and not loading even though the server didn't respond yet
  // https://www.apollographql.com/docs/react/performance/optimistic-ui/
  // We need the response to be as close as possible to th response so we need to add `optimisticResponse` here to have access to input
  const onSubmit = (input) => {
    setModal(false);
    createPet({
      variables: {
        newPet: input,
        optimisticResponse: {
          __typename: 'Mutation',
          addPet: {
            __typename: 'Pet', // < very important to add
            id: Math.floor(Math.random() * 10000) + '', // because we create a new pet we don't have an idea yet so just add a random one for now that will get updated when the real response comes.
            name: input.name,
            type: input.type,
            img: 'https://via.placeholder.com/150', // just use a placeholder
          },
        },
      },
    });
  };

  if (loading || newPet.loading) {
    return <Loader />;
  }

  if (modal) {
    return <NewPetModal onSubmit={onSubmit} onCancel={() => setModal(false)} />;
  }

  return (
    <div className="page pets-page">
      <section>
        <div className="row betwee-xs middle-xs">
          <div className="col-xs-10">
            <h1>Pets</h1>
          </div>

          <div className="col-xs-2">
            <button onClick={() => setModal(true)}>new pet</button>
          </div>
        </div>
      </section>
      <section>
        <PetsList pets={data.pets} />
      </section>
    </div>
  );
}
```

#### components/PetsList.js

```js
import React from 'react';
import PetBox from './PetBox';

export default function PetsList({ pets }) {
  return (
    <div className="row">
      {pets.map((pet) => (
        <div className="col-xs-12 col-md-4 col" key={pet.id}>
          <div className="box">
            <PetBox pet={pet} />
          </div>
        </div>
      ))}
    </div>
  );
}

PetsList.defaultProps = {
  pets: [],
};

const PetBox = ({ pet }) => (
  <div className="pet">
    <figure>
      <img src={pet.img + `?pet=${pet.id}`} alt="" />
    </figure>
    <div className="pet-name">{pet.name}</div>
    <div className="pet-type">{pet.type}</div>
  </div>
);
```

#### App.js

```js
import { Switch, Route } from 'react-router-dom';
import React, { Fragment } from 'react';
import Header from './Header';
import Pets from '../pages/Pets';

const App = () => (
  <Fragment>
    <Header />
    <div>
      <Switch>
        <Route exact path="/" component={Pets} />
      </Switch>
    </div>
  </Fragment>
);

export default App;
```

#### components/NewPetModal.js

```js
import React from 'react';

import NewPet from './NewPet';

export default function NewPetModal({ onSubmit, onCancel }) {
  return (
    <div className="row center-xs">
      <div className="col-xs-8">
        <NewPet onSubmit={onSubmit} onCancel={onCancel} />
      </div>
    </div>
  );
}
```

#### components/NewPet.js

```js
import React, { useState } from 'react';
import Select from 'react-select';

const options = [
  { value: 'CAT', label: 'Cat' },
  { value: 'DOG', label: 'Dog' },
];

export default function NewPet({ onSubmit, onCancel }) {
  const [type, setType] = useState('DOG');
  const [name, setName] = useState('');

  const activeOption = options.find((o) => o.value === type);

  const submit = (e) => {
    e.preventDefault();
    onSubmit({ name, type });
  };

  const cancel = (e) => {
    e.preventDefault();
    onCancel();
  };

  return (
    <div className="new-pet page">
      <h1>New Pet</h1>
      <div className="box">
        <form onSubmit={submit}>
          <Select
            value={activeOption}
            defaultValue={options[0]}
            onChange={(e) => setType(e.value)}
            options={options}
          />

          <input
            className="input"
            type="text"
            placeholder="pet name"
            value={name}
            onChange={(e) => setName(e.target.value)}
            required
          />
          <a className="error button" onClick={cancel}>
            cancel
          </a>
          <button type="submit" name="submit">
            add pet
          </button>
        </form>
      </div>
    </div>
  );
}
```

### Cool resources

https://apis.guru/graphql-voyager/
