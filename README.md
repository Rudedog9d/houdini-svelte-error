Short version to reproduce the error:

```shell
npx sv create --template demo --types ts svelte-houdini-error/
npx houdini@latest init
npm run dev
```

full set of steps to produce this repo:

```shell
npx sv create --template demo --types ts svelte-houdini-error/
# (edit readme with this content)

# store state in git
git init
git add .
git commit -m "init commit"

# run and verify functionality
npm run dev

# install node adapter
npm i -D @sveltejs/adapter-node
sed -i 's/@sveltejs\/adapter-auto/@sveltejs\/adapter-node/' svelte.config.js

# run and verify functionality
node build/

# store state in git
git add .
git commit -m "use node adapter"

# setup houdini - select "no remote api" and "schema.graphql" for the schema filename
npx houdini@latest init
echo 'type query_root {noop: String} schema {query: query_root}' > schema.graphql

# run app and it's broken
npm run dev

# reset build config
sed -i 's/@sveltejs\/adapter-auto/@sveltejs\/adapter-node/' svelte.config.js
rm -r build/
# build and run app
npm run build
node build/
# this actually works fine

git add .
git commit -m "setup houdini"
```

built with

```shell
# node --version
v22.12.0

# npm --version
10.9.0
```