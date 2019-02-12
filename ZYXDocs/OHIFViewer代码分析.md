# OHIFViewer/OHIFViewer代码分析

## client\routes.js

核心路由：

- Router.route('/studylist', function() -》 ohifViewer
- Router.route('/viewer/:studyInstanceUids', function() -》OHIF.viewerbase.renderViewer

## Study List查询机制

### StudylistResult

Packages\ohif-study-list\client\components\studylist\studylistResult\studylistResult.js

#### 执行搜索：function search(instance) 

```js
OHIF.studies.searchStudies(filter).then((studies)
```

#### StudyList查询：searchStudies.js

Packages\ohif-studies\imports\client\lib\searchStudies.js

```js
const server = OHIF.servers.getCurrentServer();

if (server.type === 'dicomWeb' && server.requestOptions.requestFromBrowser === true) {
    OHIF.studies.services.QIDO.Studies(server, filter).then(resolve, reject);
} else {
    Meteor.call('StudyListSearch', filter, (error, studiesData) => {
        if (error) {
            reject(error);
        } else {
            resolve(studiesData);
        }
    });
}
```

#### StudyList查询：studylistSearch.js

Packages\ohif-studies\imports\server\methods\studylistSearch.js

```js
if (server.type === 'dicomWeb') {
    return OHIF.studies.services.QIDO.Studies(server, filter);
} else if (server.type === 'dimse') {
    return OHIF.studies.services.DIMSE.Studies(filter);
}
```

#### OHIF.studies.services.QIDO.Studies

Packages\ohif-studies\imports\both\services\qido\studies.js

```js
OHIF.studies.services.QIDO.Studies = (server, filter) => {
    const config = {
        url: server.qidoRoot,
        headers: OHIF.DICOMWeb.getAuthorizationHeader()
    };

    const dicomWeb = new DICOMwebClient.api.DICOMwebClient(config);
    const queryParams = getQIDOQueryParams(filter, server.qidoSupportsIncludeField);
    const options = {
        queryParams
    };

    return dicomWeb.searchForStudies(options).then(resultDataToStudies);
};
```

#### OHIF.studies.services.DIMSE.Studies

Packages\ohif-studies\imports\server\services\dimse\studies.js

```js
    const results = DIMSE.retrieveStudies(parameters);
```

