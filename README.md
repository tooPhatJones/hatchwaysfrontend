# hatchways-frontend

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Run your tests
```
npm run test
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

### Notes for hatchways assessment
I made this app with the vue-cli
Please run npm install to load project dependecies, 
then run npm run serve to run locally
project will be available on localhost:8080

This project grabs a list of workorders from the hatchways api, and then a list of workers that work on the work orders.
Then it links the two together based on workerId and displays them in ascending order by the workorder deadline..
You can search by worker id, and the list will filter itself. Delete your search to show all orders.
The list is automatically showing in ascending order by deadline date. 
Please click the checkbox to reverse the filter by deadline.
