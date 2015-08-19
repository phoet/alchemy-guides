Page layouts
------------

endprologue.

### What are page layouts?

Every page in Alchemy has a page layout.

Think of page layouts as types of pages with different settings and
markup. They are used to define page abilities like available elements
or cells, caching settings, enabling xml feeds, and so on.

Page layouts are defined in the
<code>config/alchemy/page\_layouts.yml</code> file.

#### How do they work?

When creating a new page the user gets prompted to select a page layout.
Depending on its settings (<code>page\_layouts.yml</code>) the user has
certain elements to manage content with.

#### Rendering

Beside the page layout definitions in the <code>page\_layouts.yml</code>
file, they all have a Rails view partial which is yielded on the Rails
layout (for example on
<code>app/views/layouts/application.html.erb</code>).

They are being parsed through a template engine (ERB, haml or slim) so
you can write HTML/CSS and execute Ruby code there.

You can find all page layout partials in the
<code>app/views/alchemy/page\_layouts</code> folder.

They are named after the page layout’s name you defined in the
<code>page\_layouts.yml</code> file.

If no page layout partial is found, the <code>\_standard.html.erb</code>
file gets rendered.

INFO: Alchemy comes with a generator that creates the partials for
you.<br>\
Run <code>rails g alchemy:page\_layouts —skip</code> inside you
terminal.

#### Caching

If using the global caching option (defined in
<code>config/alchemy/config.yml</code>) which is enabled by default,
your page layouts will be cached for fast delivering through Rails
action caching.

INFO: You can disable caching of certain pages in the
<code>page\_layouts.yml</code> file by using the <code>cache:
false</code> setting.

### Create page layouts

Follow this guide to learn how to [create page
layouts](create_page_layouts.html) which can be chosen by the user when
creating a new page.

#### Defining page layouts

In order to create page layouts you have to edit the file
<code>config/alchemy/page\_layouts.yml</code>.

INFO: Generate Alchemy’s files and folders with <code>rails g
alchemy:scaffold</code>

#### Configuration options

Every page layout needs at least a name. You don’t need to set every
option. It depends on what you need for pages with that layout type.

##### Recommended options

-   <code>name:</code> <code>[String]</code><br>\
     The name of the layout used for views and inside the database. You
    can render a layout with the <code>render\_page\_layout(name)</code>
    helper.
-   <code>elements:</code> <code>[Array]</code><br>\
     A list of element names that can be placed on this layout i.e.
    <code>[text, picture]</code>.<br>\
     [Elements](elements.html) are defined inside the
    <code>elements.yml</code> file.
-   <code>autogenerate:</code> <code>[Array]</code><br>\
     A list of element names that are autogenerated after creating a
    Page of that type.
-   <code>unique:</code> <code>[Boolean]</code><br>\
     (Default false) Pass <code>true</code> and the user can only choose
    this layout once inside a language tree.
-   <code>cache:</code> <code>[Boolean]</code><br>\
     (Default true) Pass <code>false</code> to disable the caching for
    this kind of pages. <strong>Recommended for contact forms</strong>
    and such likes.

##### Additional/advanced options

-   <code>layoutpage:</code> <code>[Boolean]</code><br>\
     Layout pages (or global pages) are outside the normal page tree and
    can be used to place “global” Elements like footers and sidebars.
-   <code>hide:</code> <code>[Boolean]</code><br>\
     Pass true to hide this layout from the user.
-   <code>searchresults:</code> <code>[Boolean]</code><br>\
     Pass true to use this type of page for rendering the search results
    of the build in fulltext search.
-   <code>feed:</code> <code>[Boolean]</code><br>\
     Pass true to enable a RSS feed of news elements from this page.
-   <code>redirects\_to\_external:</code> <code>[Boolean]</code><br>\
     Pass true to disable normal page rendering and redirect to a
    external page instead.
-   <code>taggable:</code> <code>[Boolean]</code><br>\
     Pass true to be able to assign tags within page settings.
-   <code>controller:</code> <code>[String]</code><br>\
     Controller to use instead of the default Alchemy::PagesController
-   <code>action:</code> <code>[String]</code><br>\
     Controllers action to use instead of the default
    Alchemy::PagesController\#show

#### Example

Lets say you want to create a contact page with a headline element, an
e-mail formular and a text element on it.

This page should be unique, because you don’t want to give your content
manager the possibility to create more than one contact form.

This page must not be cached, because of validation messages and user
specific form content.

We also want to autogenerate the headline and the contactform element
after the page gets created.

<yaml>

1.  config/alchemy/page\_layouts.yml\
    - name: contact\
     cache: false\
     unique: true\
     elements: [headline, contactform, text]\
     autogenerate: [headline, contactform]\
    </yaml>

NOTE: <b>Please ensure to restart the server after editing
page\_layouts.yml.</b>

#### Generate the views

Every page layout defined in the <code>page\_layouts.yml</code> file
should have a view partial located in
<code>app/views/alchemy/page\_layouts/</code>.

There is no need to create these partials manually. Alchemy CMS comes
with a Rails generator task which creates these partials for you.

So after defining the page layouts, you can generate all the
corresponding partials for them.

<shell>rails g alchemy:page\_layouts —skip</shell>

Using the example above, which defines a contact layout, the generator
will create a partial named <code>\_contact.html.erb</code>.

NOTE: You can pass <code>—template-engine</code> or <code>-e</code> as
option to use one of <code>haml</code>, <code>slim</code> and
<code>erb</code>.\
The default is depending on your default template engine in your Rails
host app.

### Customize the views

Alchemy does not place any HTML markup in your generated page layouts
partial.

So:

<erb>\
\<= render\_elements\>\
</erb>

is all you will see. Feel free to customize the HTML so it fits your
needs.

INFO: Alchemy comes with tons of view helpers that you can use to render
elements.<br>\
Please have a look into the [ElementsHelper
documentation](http://rdoc.info/github/magiclabs/alchemy_cms/Alchemy/ElementsHelper#render_elements-instance_method)

#### Translate page layout names

Page layout names are passed through the <code>I18n</code> library.\
You can translate them in your Alchemy locale files.

##### Example

<yaml>

1.  config/locales/alchemy.de.yml\
    de:\
     alchemy:\
     page\_layout\_names:\
     contact: Kontakt\
     search: Suche\
    </yaml>

INFO: All translation keys used by Alchemy are scoped under the
<code>alchemy</code> namespace.

 