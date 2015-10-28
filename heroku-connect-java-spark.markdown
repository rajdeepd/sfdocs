---
layout: default
---


This sample shows how to configure a Java Spark App to work with Heroku Connect.

## Running Locally

Make sure you have Java and Maven installed.  Also, install the [Heroku Toolbelt](https://toolbelt.heroku.com/).

```sh
$ git clone https://github.com/rajdeepd/heroku-connect-java-spark-sample.git
$ cd java-getting-started
$ mvn install
$ foreman start web
```

Your app should now be running on [localhost:5000](http://localhost:5000/).

If you're going to use a database, ensure you have a local `.env` file that reads something like this:

```
DATABASE_URL=postgres://localhost:5432/java_database_name
```

## Deploying to Heroku

```sh
$ heroku create
$ git push heroku master
$ heroku open
```

## Adding Heroku Connect Add-On

Configure Heroku Connect Add-On

```sh
$ heroku addons:create herokuconnect
```

## Configure Herok Connect Add-On

* Create a Mapping with Contacts table using Heroku Dashboard

## Code for getting contacts

`/contacts` rest endpoint is tied to getting list of names from `salesforce.contact` and populating `contacts.ftl`.

```java 
 get("/contacts", (req, res) -> {
        Connection connection = null;
        Map<String, Object> attributes = new HashMap<>();
        try {
          connection = DatabaseUrl.extract().getConnection();
          Statement stmt = connection.createStatement();

          ResultSet rs = stmt.executeQuery("SELECT name FROM salesforce.contact");

          ArrayList<String> output = new ArrayList<String>();
          while (rs.next()) {
            output.add( rs.getString("name"));
          }
          attributes.put("results", output);
          return new ModelAndView(attributes, "contacts.ftl");
        } catch (Exception e) {
          attributes.put("message", "There was an error: " + e);
          return new ModelAndView(attributes, "error.ftl");
        } finally {
          if (connection != null) try{connection.close();} catch(SQLException e){}
        }
      }, new FreeMarkerEngine());
```
## Show Contacts
 Browse to URL `http://{your-app-name}.herokuapp.com/contacts` to see the list of contact names.

In the Developer Settings section, click "edit".

<img src="images/packages-all-about-packages-step2.png" width="600px" />

Read the details about packages and click "continue".

<img src="images/packages-namespace-check-availability-step3.png" width="600px" />

Enter the namespace prefix you would like to use and click "Check Availability". When the namespace you want shows as available, click on "Review My Selections."

<img src="images/packages-namespace-save-step4.png" width="600px" />

If everything looks correct, click "Save".

<img src="images/packages-namespace-done-step5.png" width="600px" />

That's it -- you now have a namespace.



<a name="auradocs"></a>

###Step 4: Get to Know the Docs ###

There are two main sources of documentation: the Lightning Developer Guide and AuraDocs.

You can find the Lightning Developer Guide [online here](https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/) or [download the PDF](http://www.salesforce.com/us/developer/docs/lightning/lightning.pdf). I downloaded the PDF.

The AuraDocs are a little different than the Salesforce docs you might be used to. Instead of being available in the same way as your Apex and Visualforce docs, they're actually their own app inside your org. 

<img src="images/lightning-components-auradocs.png" width="600px" />

This is incredibly cool because they aren't just hosted in your org, they're running in your org. That's right: they're an app and they'll update to include the components you build. See the "Reid002" namespace in the screenshot? Those are the apps, components and events I created while writing this article.

Access your live AuraDocs at <tt>https://{YourInstance}.lightning.force.com/auradocs</tt>.

##Next: [Hello Salesforce1](hello-salesforce1.html)##
