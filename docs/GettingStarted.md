# Getting Started
Do a `npm install ladda-cache --save` in your project. Now, to use Ladda you need to configure it and export an API built from the configuration. Create a file "api/index.js":

```javascript
import * as projectApi from './project';
import { build } from 'ladda-cache';

const config = {
    project: {
        api: projectApi
    }
};

export default build(config);
```

where project is a bunch of api-methods (living in /api/project.js) returning promises, it might look like:

```javascript
getProjects.operation = 'READ';
export function getProjectsCreatedAfter(date) {
    return get(resource, {date}); // Returns a list of projects (where each project has an ID)
}
```
where get is a function preforming a HTTP-requests and returning a promise. You will need to create an API for your own application using your own method for creating get requests (for example Axios). When you call `getProjectsCreatedAfter` the results will be cached. So if you call it more than once within 300s (default time to life for the cache), only one HTTP-request will be made per date. Eg. 

```
getProjectsCreatedAfter('2000-01-01');
getProjectsCreatedAfter('2000-01-01');
getProjectsCreatedAfter('2010-01-01');
getProjectsCreatedAfter('2010-01-01');
```
Would result in two requests, one for 2000-01-01 and one for 2010-01-01.