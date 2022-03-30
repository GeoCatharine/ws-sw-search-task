# search app (websupport)

Interview task about Star Wars characters search, build with Vue.js v2, Quasar CLI and Swapi - Star Wars API.

I opted to not use Typescript, mostly because i ran into some issues between Typescript and VScode early on and due to lack of time i decided to speedrun my way without it, however i might go back and try to fix it in my spare time ```just for my good feeling```.

The biggest issue i met was bold characters in api list that match searched input text. 
I never made my own filter and had to take alot of inspiration from google to figure out how RegExp work so i can build it from scratch.
And im keeping this one for future in separate file.
```even tho im pretty sure theres library for it somewhere out there ```

Second challenge for me was little background highlight of searched text when there is no match in api.
```im not even sure if that was part of task or just some browser setting```
I met this issue before and "hacked" my way throu it, however this time that fix didnt fit circumstances. But it was Quasar that saved me with their slots in inputs. I just copied input text to the slot, position it right behind input text and set text color to transparent so it imitated the input and background color to slight red.
``` there is library for it, i opted to not use it, i might like pain ```
Only issue i noticed here is long latency between search input and slot, that might be due to Quasar debounce, but im not sure and will look into it further.



## Install the dependencies
```bash
yarn
# or
npm install
```

### Start the app in development mode (hot-code reloading, error reporting, etc.)
```bash
quasar dev
```


### Lint the files
```bash
yarn lint
# or
npm run lint
```

### Build the app for production
```bash
quasar build
```

### Customize the configuration
See [Configuring quasar.conf.js](https://v1.quasar.dev/quasar-cli/quasar-conf-js).
