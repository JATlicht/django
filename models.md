---


---

<h3 id="models-and-orm-in-django">Models and ORM in Django</h3>
<p>Django’s ORM provides a powerful and flexible way to interact with the database, allowing you to define your database schema entirely in Python code.</p>
<h4 id="creating-models">1. Creating Models</h4>
<p>Models are Python classes that represent database tables. Each model maps to a single database table, and each attribute of the model represents a database field.</p>
<p>Let’s create a simple blog application with a <code>Post</code> model.</p>
<ol>
<li>
<p><strong>Create a new application</strong>:</p>
<p>bash</p>
<p>Copy code</p>
<p><code>python manage.py startapp blog</code></p>
</li>
<li>
<p><strong>Define the <code>Post</code> model</strong>: In <code>blog/models.py</code>:</p>
<p>python</p>
<p>Copy code</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token keyword">from</span> django<span class="token punctuation">.</span>db <span class="token keyword">import</span> models

<span class="token keyword">class</span> <span class="token class-name">Post</span><span class="token punctuation">(</span>models<span class="token punctuation">.</span>Model<span class="token punctuation">)</span><span class="token punctuation">:</span>
    title <span class="token operator">=</span> models<span class="token punctuation">.</span>CharField<span class="token punctuation">(</span>max_length<span class="token operator">=</span><span class="token number">200</span><span class="token punctuation">)</span>
    content <span class="token operator">=</span> models<span class="token punctuation">.</span>TextField<span class="token punctuation">(</span><span class="token punctuation">)</span>
    published_date <span class="token operator">=</span> models<span class="token punctuation">.</span>DateTimeField<span class="token punctuation">(</span>auto_now_add<span class="token operator">=</span><span class="token boolean">True</span><span class="token punctuation">)</span>

    <span class="token keyword">def</span> <span class="token function">__str__</span><span class="token punctuation">(</span>self<span class="token punctuation">)</span><span class="token punctuation">:</span>
        <span class="token keyword">return</span> self<span class="token punctuation">.</span>title
</code></pre>
<ul>
<li><code>CharField</code>: A short-to-mid-sized string.</li>
<li><code>TextField</code>: A large text field.</li>
<li><code>DateTimeField</code>: A date and time field, with <code>auto_now_add</code> setting the field to the current date and time when the object is created.</li>
</ul>
</li>
<li>
<p><strong>Register the model with the admin site</strong>: In <code>blog/admin.py</code>:</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token keyword">from</span> django<span class="token punctuation">.</span>contrib <span class="token keyword">import</span> admin
<span class="token keyword">from</span> <span class="token punctuation">.</span>models <span class="token keyword">import</span> Post

admin<span class="token punctuation">.</span>site<span class="token punctuation">.</span>register<span class="token punctuation">(</span>Post<span class="token punctuation">)</span>
</code></pre>
</li>
</ol>
<h4 id="making-migrations">2. Making Migrations</h4>
<p>After defining your models, you need to create and apply migrations to update the database schema.</p>
<ol>
<li>
<p><strong>Create migration files</strong>:</p>
<p>bash<br>
<code>python manage.py makemigrations blog</code></p>
</li>
<li>
<p><strong>Apply migrations to update the database</strong>:</p>
<p>bash<br>
<code>python manage.py migrate</code></p>
</li>
</ol>
<h4 id="interacting-with-models">3. Interacting with Models</h4>
<p>You can interact with your models through Django’s ORM.</p>
<ol>
<li>
<p><strong>Open the Django shell</strong>:</p>
<p>bash<br>
<code>python manage.py shell</code></p>
</li>
<li>
<p><strong>Create a new post</strong>:</p>
<p>python</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token keyword">from</span> blog<span class="token punctuation">.</span>models <span class="token keyword">import</span> Post
new_post <span class="token operator">=</span> Post<span class="token punctuation">(</span>title<span class="token operator">=</span><span class="token string">'My First Post'</span><span class="token punctuation">,</span> content<span class="token operator">=</span><span class="token string">'This is the content of my first post.'</span><span class="token punctuation">)</span>
new_post<span class="token punctuation">.</span>save<span class="token punctuation">(</span><span class="token punctuation">)</span>
</code></pre>
</li>
<li>
<p><strong>Query posts</strong>:</p>
<p>python</p>
<p>Copy code</p>
<pre class=" language-python"><code class="prism  language-python">all_posts <span class="token operator">=</span> Post<span class="token punctuation">.</span>objects<span class="token punctuation">.</span><span class="token builtin">all</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
<span class="token keyword">print</span><span class="token punctuation">(</span>all_posts<span class="token punctuation">)</span>
</code></pre>
</li>
<li>
<p><strong>Filter posts</strong>:</p>
<p>python</p>
<p>Copy code</p>
<p>`python<br>
first_post = Post.objects.get(id=1)<br>
print(first_post.title)</p>
<pre><code>

</code></pre>
</li>
</ol>
<h4 id="displaying-posts-in-a-view">4. Displaying Posts in a View</h4>
<p>Now let’s create a view to display the posts.</p>
<ol>
<li>
<p><strong>Create a view in <code>blog/views.py</code></strong>:</p>
<p>python</p>
<p>Copy code</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token keyword">from</span> django<span class="token punctuation">.</span>shortcuts <span class="token keyword">import</span> render
<span class="token keyword">from</span> <span class="token punctuation">.</span>models <span class="token keyword">import</span> Post

<span class="token keyword">def</span> <span class="token function">post_list</span><span class="token punctuation">(</span>request<span class="token punctuation">)</span><span class="token punctuation">:</span>
    posts <span class="token operator">=</span> Post<span class="token punctuation">.</span>objects<span class="token punctuation">.</span><span class="token builtin">all</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
    <span class="token keyword">return</span> render<span class="token punctuation">(</span>request<span class="token punctuation">,</span> <span class="token string">'blog/post_list.html'</span><span class="token punctuation">,</span> <span class="token punctuation">{</span><span class="token string">'posts'</span><span class="token punctuation">:</span> posts<span class="token punctuation">}</span><span class="token punctuation">)</span>
</code></pre>
</li>
<li>
<p><strong>Create a URL pattern in <code>blog/urls.py</code></strong>:</p>
<p>python</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token keyword">from</span> django<span class="token punctuation">.</span>urls <span class="token keyword">import</span> path
<span class="token keyword">from</span> <span class="token punctuation">.</span> <span class="token keyword">import</span> views

urlpatterns <span class="token operator">=</span> <span class="token punctuation">[</span>
    path<span class="token punctuation">(</span><span class="token string">''</span><span class="token punctuation">,</span> views<span class="token punctuation">.</span>post_list<span class="token punctuation">,</span> name<span class="token operator">=</span><span class="token string">'post_list'</span><span class="token punctuation">)</span><span class="token punctuation">,</span>
<span class="token punctuation">]</span>
</code></pre>
</li>
<li>
<p><strong>Include the blog URLs in the project’s <code>urls.py</code></strong>: In <code>project/urls.py</code>:</p>
<p>python</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token keyword">from</span> django<span class="token punctuation">.</span>contrib <span class="token keyword">import</span> admin
<span class="token keyword">from</span> django<span class="token punctuation">.</span>urls <span class="token keyword">import</span> path<span class="token punctuation">,</span> include

urlpatterns <span class="token operator">=</span> <span class="token punctuation">[</span>
    path<span class="token punctuation">(</span><span class="token string">'admin/'</span><span class="token punctuation">,</span> admin<span class="token punctuation">.</span>site<span class="token punctuation">.</span>urls<span class="token punctuation">)</span><span class="token punctuation">,</span>
    path<span class="token punctuation">(</span><span class="token string">''</span><span class="token punctuation">,</span> include<span class="token punctuation">(</span><span class="token string">'blog.urls'</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">,</span>
<span class="token punctuation">]</span>
</code></pre>
</li>
<li>
<p><strong>Create a template to display posts</strong>: In <code>blog/templates/blog/post_list.html</code>:</p>
<p>html</p>
<p>Copy code</p>
<pre class=" language-html"><code class="prism  language-html"><span class="token doctype">&lt;!DOCTYPE html&gt;</span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>html</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>head</span><span class="token punctuation">&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>title</span><span class="token punctuation">&gt;</span></span>Blog Posts<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>title</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>head</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>body</span><span class="token punctuation">&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>h1</span><span class="token punctuation">&gt;</span></span>Blog Posts<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>h1</span><span class="token punctuation">&gt;</span></span>
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>ul</span><span class="token punctuation">&gt;</span></span>
        {% for post in posts %}
            <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>li</span><span class="token punctuation">&gt;</span></span>
                <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>h2</span><span class="token punctuation">&gt;</span></span>{{ post.title }}<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>h2</span><span class="token punctuation">&gt;</span></span>
                <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>p</span><span class="token punctuation">&gt;</span></span>{{ post.content }}<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>p</span><span class="token punctuation">&gt;</span></span>
                <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>p</span><span class="token punctuation">&gt;</span></span><span class="token tag"><span class="token tag"><span class="token punctuation">&lt;</span>em</span><span class="token punctuation">&gt;</span></span>{{ post.published_date }}<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>em</span><span class="token punctuation">&gt;</span></span><span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>p</span><span class="token punctuation">&gt;</span></span>
            <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>li</span><span class="token punctuation">&gt;</span></span>
        {% endfor %}
    <span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>ul</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>body</span><span class="token punctuation">&gt;</span></span>
<span class="token tag"><span class="token tag"><span class="token punctuation">&lt;/</span>html</span><span class="token punctuation">&gt;</span></span>
</code></pre>
</li>
</ol>
<p>Now, when you run your Django development server, you should see a list of blog posts on the homepage.</p>
<h3 id="basic-field-types">Basic Field Types</h3>
<ol>
<li>
<p><strong>CharField</strong>: A short-to-mid-sized string for small to medium-sized text.<br>
<code>name = models.CharField(max_length=100)</code></p>
</li>
<li>
<p><strong>TextField</strong>: A large text field for long text.<br>
<code>description = models.TextField()</code><br>
3.  <strong>IntegerField</strong>: An integer field.<br>
<code>age = models.IntegerField()</code></p>
</li>
<li>
<p><strong>FloatField</strong>: A floating-point number.</p>
<p><code>price = models.FloatField()</code></p>
</li>
<li>
<p><strong>DecimalField</strong>: A fixed-precision decimal number.</p>
<p><code>salary = models.DecimalField(max_digits=10, decimal_places=2)</code></p>
</li>
<li>
<p><strong>BooleanField</strong>: A true/false field.</p>
<p><code>is_active = models.BooleanField(default=True)</code></p>
</li>
</ol>
<h3 id="date-and-time-fields">Date and Time Fields</h3>
<ol>
<li>
<p><strong>DateField</strong>: A date field.</p>
<p><code>birth_date = models.DateField()</code></p>
</li>
<li>
<p><strong>TimeField</strong>: A time field.</p>
<p><code>appointment_time = models.TimeField()</code></p>
</li>
<li>
<p><strong>DateTimeField</strong>: A date and time field.</p>
<p><code>created_at = models.DateTimeField(auto_now_add=True)</code></p>
</li>
<li>
<p><strong>DurationField</strong>: A field for storing periods of time.</p>
<p><code>duration = models.DurationField()</code></p>
</li>
</ol>
<h3 id="file-fields">File Fields</h3>
<ol>
<li>
<p><strong>FileField</strong>: A file-upload field.</p>
<p><code>file = models.FileField(upload_to='uploads/')</code></p>
</li>
<li>
<p><strong>ImageField</strong>: An image-upload field.</p>
<p><code>image = models.ImageField(upload_to='images/')</code></p>
</li>
</ol>
<h3 id="numeric-fields">Numeric Fields</h3>
<ol>
<li>
<p><strong>BigIntegerField</strong>: A large integer field.</p>
<p><code>large_number = models.BigIntegerField()</code></p>
</li>
<li>
<p><strong>PositiveIntegerField</strong>: An integer field that must be positive.</p>
<p><code>positive_number = models.PositiveIntegerField()</code></p>
</li>
<li>
<p><strong>PositiveSmallIntegerField</strong>: A small integer field that must be positive.</p>
<p><code>small_positive_number = models.PositiveSmallIntegerField()</code></p>
</li>
</ol>
<h3 id="relation-fields">Relation Fields</h3>
<ol>
<li>
<p><strong>ForeignKey</strong>: A foreign key relationship.</p>
<p><code>author = models.ForeignKey('Author', on_delete=models.CASCADE)</code></p>
</li>
<li>
<p><strong>OneToOneField</strong>: A one-to-one relationship.</p>
<p><code>profile = models.OneToOneField('Profile', on_delete=models.CASCADE)</code></p>
</li>
<li>
<p><strong>ManyToManyField</strong>: A many-to-many relationship.</p>
<p><code>categories = models.ManyToManyField('Category')</code></p>
</li>
</ol>
<h3 id="other-common-fields">Other Common Fields</h3>
<ol>
<li>
<p><strong>EmailField</strong>: A field for email addresses.</p>
<p><code>email = models.EmailField()</code></p>
</li>
<li>
<p><strong>URLField</strong>: A field for URLs.</p>
<p><code>website = models.URLField()</code></p>
</li>
<li>
<p><strong>SlugField</strong>: A field for storing slugs (usually used in URLs).</p>
<p><code>slug = models.SlugField()</code></p>
</li>
<li>
<p><strong>UUIDField</strong>: A field for storing universally unique identifiers.</p>
<p><code>uuid = models.UUIDField(default=uuid.uuid4, editable=False, unique=True)</code></p>
</li>
<li>
<p><strong>BinaryField</strong>: A field for storing raw binary data.</p>
<p><code>binary_data = models.BinaryField()</code></p>
</li>
</ol>
<h3 id="example-model-using-various-field-types">Example Model Using Various Field Types</h3>
<p>Here’s an example of a model that uses several different field types:</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token keyword">from</span> django<span class="token punctuation">.</span>db <span class="token keyword">import</span> models
<span class="token keyword">import</span> uuid

<span class="token keyword">class</span> <span class="token class-name">ExampleModel</span><span class="token punctuation">(</span>models<span class="token punctuation">.</span>Model<span class="token punctuation">)</span><span class="token punctuation">:</span>
	name <span class="token operator">=</span> models<span class="token punctuation">.</span>CharField<span class="token punctuation">(</span>max_length<span class="token operator">=</span><span class="token number">100</span><span class="token punctuation">)</span>
    description <span class="token operator">=</span> models<span class="token punctuation">.</span>TextField<span class="token punctuation">(</span><span class="token punctuation">)</span>
    age <span class="token operator">=</span> models<span class="token punctuation">.</span>IntegerField<span class="token punctuation">(</span><span class="token punctuation">)</span>
    price <span class="token operator">=</span> models<span class="token punctuation">.</span>FloatField<span class="token punctuation">(</span><span class="token punctuation">)</span>
    salary <span class="token operator">=</span> models<span class="token punctuation">.</span>DecimalField<span class="token punctuation">(</span>max_digits<span class="token operator">=</span><span class="token number">10</span><span class="token punctuation">,</span> decimal_places<span class="token operator">=</span><span class="token number">2</span><span class="token punctuation">)</span>
    is_active <span class="token operator">=</span> models<span class="token punctuation">.</span>BooleanField<span class="token punctuation">(</span>default<span class="token operator">=</span><span class="token boolean">True</span><span class="token punctuation">)</span>
    birth_date <span class="token operator">=</span> models<span class="token punctuation">.</span>DateField<span class="token punctuation">(</span><span class="token punctuation">)</span>
    created_at <span class="token operator">=</span> models<span class="token punctuation">.</span>DateTimeField<span class="token punctuation">(</span>auto_now_add<span class="token operator">=</span><span class="token boolean">True</span><span class="token punctuation">)</span>
    <span class="token builtin">file</span> <span class="token operator">=</span> models<span class="token punctuation">.</span>FileField<span class="token punctuation">(</span>upload_to<span class="token operator">=</span><span class="token string">'uploads/'</span><span class="token punctuation">)</span>
    image <span class="token operator">=</span> models<span class="token punctuation">.</span>ImageField<span class="token punctuation">(</span>upload_to<span class="token operator">=</span><span class="token string">'images/'</span><span class="token punctuation">)</span>
    large_number <span class="token operator">=</span> models<span class="token punctuation">.</span>BigIntegerField<span class="token punctuation">(</span><span class="token punctuation">)</span>
    email <span class="token operator">=</span> models<span class="token punctuation">.</span>EmailField<span class="token punctuation">(</span><span class="token punctuation">)</span>
    website <span class="token operator">=</span> models<span class="token punctuation">.</span>URLField<span class="token punctuation">(</span><span class="token punctuation">)</span>
    slug <span class="token operator">=</span> models<span class="token punctuation">.</span>SlugField<span class="token punctuation">(</span><span class="token punctuation">)</span>
    uuid <span class="token operator">=</span> models<span class="token punctuation">.</span>UUIDField<span class="token punctuation">(</span>default<span class="token operator">=</span>uuid<span class="token punctuation">.</span>uuid4<span class="token punctuation">,</span> editable<span class="token operator">=</span><span class="token boolean">False</span><span class="token punctuation">,</span> unique<span class="token operator">=</span><span class="token boolean">True</span><span class="token punctuation">)</span>
    binary_data <span class="token operator">=</span> models<span class="token punctuation">.</span>BinaryField<span class="token punctuation">(</span><span class="token punctuation">)</span>

    <span class="token keyword">def</span> <span class="token function">__str__</span><span class="token punctuation">(</span>self<span class="token punctuation">)</span><span class="token punctuation">:</span>
        <span class="token keyword">return</span> self<span class="token punctuation">.</span>name
</code></pre>
<hr>
<h3 id="benefits-of-get_absolute_url">Benefits of <code>get_absolute_url</code></h3>
<ol>
<li><strong>Consistency</strong>: Ensures that there is a single, consistent way to generate the URL for an object.</li>
<li><strong>DRY Principle</strong>: Avoids repeating URL generation logic in multiple places in your code.</li>
<li><strong>Ease of Use</strong>: Allows you to easily link to an object’s detail view in templates and other parts of your application.</li>
</ol>
<h3 id="example-of-get_absolute_url">Example of <code>get_absolute_url</code></h3>
<p>Here’s how you can define <code>get_absolute_url</code> in your model:</p>

