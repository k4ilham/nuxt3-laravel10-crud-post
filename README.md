npx nuxi@3.2.3 init nuxt3-crud 
npm install
npm run dev -- -oy

npx nuxi add layout default
npx nuxi add page index
npx nuxi add page "posts/index"
npx nuxi add page "posts/create/index"
npx nuxi add page "posts/edit/[id]"
