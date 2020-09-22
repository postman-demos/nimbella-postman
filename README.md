# From APIs to Serverless Cloud Applications in Minutes

This is a hands-on project to support [a joint Nimbella and Postman
webinar on September 23rd, 2020](https://www.postman.com/webinars/nimbella/).
You are welcome to follow along during the webinar, and once the webinar is done we'll
post the link to the video here so that anyone can continue to learn
how to deploy serverless APIs to the cloud using Postman and Nimbella.

## Base Requirements
To be able to get hands on with this project you will need two things:

- [Postman](https://www.postman.com/) - You will need to have signed up and downloaded Postman.
- [Nimbella](https://nimbella.com/signup) - You will need to have signed up for Nimbella and downloaded the CLI.

**Authenticate**:
```
nim login
```

**Install Postman Plugin**:
```
nim plugins add postman
```

**Postman Key**: [From Your Account](https://go.postman.co/integrations/services/pm_pro_api)
- You will need your Postman API key when generating a project for your APIs on your machine.
- You will be prompted to enter the API key when it is needed.
- You can configure the Nimbella CLI to use your Postman API key by running the following command. The key is only stored locally on your machine.
```
nim auth postman --add --name mykey --key <Postman API key>
```

Now that you have Postman and Nimbella installed, are authenticated,
and added the Postman plugin you should be ready to deploy your
serverless API to the cloud.

## Deploying Serverless API
You can choose from three separate types of APIs to deploy a
serverless API from Postman using Nimbella, or use your Postman
collection.

___

### Products
A simple products API using the Schema.org products object — click to run in your Postman.

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/5c58b258f4940254570b)

**Create Project**:
```
nim project create -s postman -i Products --language js
```

**Deploy Project**:
```
nim project deploy products
```

- You should see a project created locally on your machine in a directory called `products`.
- In the root of the `products` directory, there is a Postman collection.
- Go ahead an import it into Postman, and you should be able to use your new API.

We created 4 branches which incrementally implement the APIs in this example. We also added a simple Web page to complete the project. You can deploy each of these branchs yourself directly from the GitHub repo as shown in the commands below. You can run these commands from your terminal, and even from the [Nimbella Workbench](https://apigcp.nimbella.io/wb).

1. [`step0-nimbella-project`](https://github.com/rabbah/nimbella-postman/tree/step0-nimbella-project): The Nimbella project resulting from the `nim project create` command above.
```
nim project deploy github:rabbah/nimbella-postman/products#step0-nimbella-project
```
2. [`step1-add-product`](https://github.com/rabbah/nimbella-postman/tree/step1-add-product): Implements `POST /products` to add a product to a Nimbella integrated database.
```
nim project deploy github:rabbah/nimbella-postman/products#step1-add-product
```
3. [`step2-get-products`](https://github.com/rabbah/nimbella-postman/tree/step2-get-products): Implements `GET /products` to list all added products.
```
nim project deploy github:rabbah/nimbella-postman/products#step2-get-products
```
4. [`step3-get-single-product`](https://github.com/rabbah/nimbella-postman/tree/step3-get-single-product): Implements `GET /products/:productid` to get a specific product.
```
nim project deploy github:rabbah/nimbella-postman/products#step3-get-single-product
```
5. [`step4-web-ui`](https://github.com/rabbah/nimbella-postman/tree/step4-web-ui): Add a simple web page to render the added products.
```
nim project deploy github:rabbah/nimbella-postman/products#step4-web-ui
```

When you have completed the last step, open your browser to the URL shown in the deploy message to see the results.

___

### Persons
A simple products API using the Schema.org products object — click to run in your Postman.

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/89cc94a608f66bee4b1e)

**Create Project**:
```
nim project create -s postman -i Persons --language js
```

**Deploy Project**:
```
nim project deploy persons
```

- You should see a project created locally on your machine in a directory called `persons`.
- In the root of the `persons` directory, there is a Postman collection.
- Go ahead an import it into Postman, and you should be able to use your new API.

___

### Blogs
A simple products API using the Schema.org products object — click to run in your Postman.

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/706da016ef0fae61a664)

**Create Project**:
```
nim project create -s postman -i Blogs --language js
```

**Deploy Project**:
```
nim project deploy blogs
```

- You should see a project created locally on your machine in a directory called `blogs`.
- In the root of the `blogs` directory, there is a Postman collection.
- Go ahead an import it into Postman, and you should be able to use your new API.

___

## Finishing Touches

Your API should work, returning the sample data available in the
collection, but you will need to do the work to connect your API to a
database, and customize how it works. When ready, after you have made
each change, go ahead and run `nim project deploy .` (assuming you are
in the project directory) again to publish your API.

## Testing Your APIs

Once you have your API published to the cloud using Postman and
Nimbella, and imported the resulting Postman collection, you can begin
adding tests to each of the requests, to verify that your API is
always working as expected. You can add these scripts using the
"tests" tab for each request in your collection.

### Check for 201 HTTP Status on POST

We want to make sure a 201 status code is returned for our POST.
```js
// Begin Test - 201 Status
pm.test(test_name, function () {
     pm.response.to.have.status(201);
});
// End Test
```

### Check for 200 HTTP Status on GET

We want to make sure a 200 status code is returned for our GET requests.
```js
// Begin Test - 200 Status
pm.test(test_name, function () {
    pm.response.to.have.status(200);
});
// End Test
```

### Check for 204 HTTP Status on PUT and DELETE

We want to make sure a 201 status code is returned for our PUT and DELETE.
```js
// Begin Test - 204 Status
pm.test(test_name, function () {
    pm.response.to.have.status(204);
});
// End Test
```

### Check for No Body HTTP Status on POST, PUT and DELETE

For our POST, PUT, and DELETE we want to make sure an empty body is returned.
```js
// Begin Test - No Body
var test_name = 'No Body?';
pm.test(test_name, function () {
    pm.response.to.not.have.jsonBody("");
});
// Begin Test
```

### Check for valid JSON on our GET requests

Both the the GET requests should always return valid JSON.
```js
// Begin Test - Valid JSON
var test_name = 'Is valid JSON?';
contract_tests[date_stamp][request_name][test_name] = false;
pm.test(test_name, function () {
    pm.response.to.be.json;
    contract_tests[date_stamp][request_name][test_name] = true;
});
// End Test
```

This provides us with the foundation of our API testing, of course we
can go further, checking for specific values within the JSON, but this
should demonstrate what is possible.

## Monitoring Your APIs

Now that we have our APIs up and running, and a collection that will
test each of the individual paths, we can run our collection on a
schedule as a Postman monitor. Just click on the ... action to the
right of your collection, and you will be given a configuration screen
for your monitor.

![Image of Postman monitor](https://kinlane-productions2.s3.amazonaws.com/postman-tutorials/nimbella/nimbella-postman-monitor.png)

## Support

If you have any questions feel free to submit a GitHub issue for this
repository, and we will try and support you in any way we can. Thanks
for tuning in, hopefully it helps you see the potential of deploying
APIs using Postman and Nimbella.
