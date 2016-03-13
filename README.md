# Morpheus

Morpheus is a [JSONAPI](http://jsonapi.org/) deserializer for android that uses java reflection.
You can define your own java classes to deserialize.

Take a look at the [documentation](http://www.raphaelseher.at/Morpheus/docs/index.html).

## Usage

Prepare your resources

1. extend Resource
2. Use @SerializeName() annotation when your field name differs from the json.
3. Create relationship mapping with the @Relationship() annotation.

```java
public class Article extends Resource {
  @SerializeName("article-title")
  private String title;
  @Relationship("author")
  private Author author;
  @Relationship("comments")
  private List<Comment> comments;
}
```
Deserialize your data

1. Create a Morpheus instance
2. Register your resources
3. parse your JSON string

```java
Morpheus morpheus = new Morpheus();
//register your resources
Deserializer.registerResourceClass("articles", Article.class);
Deserializer.registerResourceClass("people", Author.class);
Deserializer.registerResourceClass("comments", Comment.class);
JSONAPIObject jsonapiObject =
        morpheus.parse(loadJSONFromAsset(R.raw.articles));
        
Article article = (Article)jsonapiObject.getResources().get(0);
Log.v(TAG, "Article Id: " + article.getId())
```

# Development status
Morpheus can (0.3.0):

* deserialize data object or array
* deserialize relationships
* map includes to relationships
* deserialize links, meta, errors

# Data Attribute Mapping
At the moment Morpheus maps

* Strings -> `String`
* Floats -> `double`
* Booleans -> `boolean`
* JSONArrays -> `List<Object>`
* JSONObject -> `ArrayMap<String, Object>`


You can write your own AttributeMapper by extending `AttributeMapper.java` and initialize Morpheus with your mapper.

# Contribution
If you want to contritbute: make your changes and do a pull request.
