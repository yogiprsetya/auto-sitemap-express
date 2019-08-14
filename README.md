## Install NPM

`npm install sitemap express-list-endpoints --save`

## server.js
```
const express = require('express'),
	sm = require('sitemap'),
	listEndpoints = require('express-list-endpoints');

var app = express();
```
Create object to save all listed endoints
```
var  pathList  = [];

for(let i = 0; i < listEndpoints(app).length; i++) {
	pathList[i] = {url: listEndpoints(app)[i].path}
}
```
Define your sitemap conf

```
var sitemap = sm.createSitemap({
	hostname: http://localhost:8080,
	cacheTime: 600000,
	urls: pathList
});
```
Create sitemap page

```
app.get('/sitemap.xml', function(req, res) {
	sitemap.toXML(function (err, xml) {
		if(err) {
			return res.status(500).end();
		}
		res.header('Content-Type', 'application/xml');
		res.send(xml);
	});
});
```
