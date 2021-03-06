Blogging for Administrators
===========================


Installation
------------

The extension needs to be installed as any other extension of TYPO3 CMS:

#. Switch to the module “Extension Manager”.

#. Get the extension

   #. **Get it from the Extension Manager:** Press the “Retrieve/Update”
      button and search for the extension key *blog* and import the
      extension from the repository.

   #. **Get it from typo3.org:** You can always get current version from
      `https://typo3.org/extensions/repository/view/blog/current/
      <https://typo3.org/extensions/repository/view/blog/current/>`_ by
      downloading either the t3x or zip version. Upload
      the file afterwards in the Extension Manager.

   #. **Use composer**: Use `composer require T3G/blog`.


Latest version from git
-----------------------
You can get the latest version from git by using the git command:

.. code-block:: bash

   git clone https://github.com/TYPO3GmbH/blog.git


Setup
-----

Use the Setup Wizard
^^^^^^^^^^^^^^^^^^^^

The Setup Wizard creates the recommended pagetree and it will add all configurations and plugins you need.

To create a new blog setup, follow these steps:

Step 1. On the far left menu, clic on Setup in the Blog section, like shown below.

.. image:: ../Images/Backend/blog_setup.png

Step 2. In the SetupWizard, clic on the blue Setup a new blog button.

.. image:: ../Images/Backend/setupwizard.png

Step 3. Provide a name for the new blog and clic on Setup.

.. image:: ../Images/Backend/title_of_blog.png

If the success message appears, the setup is done. Go to your page tree (maybe reload the tree) and you will see the generated page structure.

.. image:: ../Images/Backend/page_tree.png

The Setup Wizard creates a standalone instance with the following page structure for you:

* Rootpage (hidden by default, contains the TypoScript and PageTS-Config)
* Data (a folder to hold categories, authors and tags, but also blog posts are possible)
* Category (this page is used to show blog posts, related to single category, or a category overview)
* Tag (this page is used to show blog posts, related to single tag, or a tag overview)
* Archive (this page is the archive, it lists all blog posts by given date (month and year, or year only)
* First blog post (yes, a first blog post, as an example)

The Setup Wizard does also take care of all your configuration needs. I fills in the constants, inserts the static template and the Page TSconfig too.

.. image:: ../Images/Backend/constants.png

The Setup Wizard performs a fully configured standalone instance of the Blog extension. If you already have an existenig site, you might dislike the result of having an additional and unplanned root page. In that case you should read about the manual setup in the next section.


Setup without Wizard
^^^^^^^^^^^^^^^^^^^^

This is the setup method you will need in order to integrate the Blog extension into an existing site. If you want a standalone Blog and do not have an existing site, please go with the Setup Wizard instructions in the prior section.

To create a new blog setup, follow these steps:

Step 1. Create the following page structure:

.. image:: ../Images/Backend/pagetree_no_wizard.png

In the picture above, the existing site is represented by the pages with ids 1 through 5.

* A standard page (id = 6) is the root page of the blog tree. It holds the Page TSconfig.
* Data (a folder to hold categories, authors and tags, but also blog posts are possible)
* Category (this page is used to show blog posts, related to single category, or a category overview)
* Tag (this page is used to show blog posts, related to single tag, or a tag overview)
* Archive (this page is the archive, it lists all blog posts by given date (month and year, or year only

Step 2. Configure the page ids in the constants. This constants go either in the constants of the root template (id=1) or, even better, in the constants.typoscript file in the sitepackage.

.. code-block:: ts

   plugin.tx_blog.settings.blogUid = NEW_blogRoot
   plugin.tx_blog.settings.categoryUid = NEW_blogCategoryPage
   plugin.tx_blog.settings.authorUid = NEW_blogAuthorPage
   plugin.tx_blog.settings.tagUid = NEW_blogTagPage
   plugin.tx_blog.settings.archiveUid = NEW_blogArchivePage
   plugin.tx_blog.settings.storagePid = NEW_blogFolder

Step 3. In the template of the root page of the site (id=1), include the static template. Please go with either Integration or Expert template.

Frontend view of blog post list.

Backend view of blog post list.
Step 4. The root page of the blog tree, in this case the page with id = 6, will carry the Page TSconfig entries. These point all to the storage folder, in this case id = 7.

.. code-block:: ts

   TCEFORM.pages.tags.PAGE_TSCONFIG_ID = 7
   TCEFORM.pages.authors.PAGE_TSCONFIG_ID = 7
   TCEFORM.pages.categories.PAGE_TSCONFIG_ID = 7

This rounds up the manual installation method.

**Note:** If you have multiple folder or root pages for your blog posts your have to add all root pages to :typoscript:`plugin.tx_blog.settings.storagePid` as a comma separated list. The first value must be the value of NEW_blogFolder


Frontend Routing Setup
^^^^^^^^^^^^^^^^^^^^^^

The extension provides a frontend route enhancer configuration that you can include it in your site configuration.

.. code-block:: yaml

   imports:
     - { resource: "EXT:blog/Configuration/Routes/Default.yaml" }

Feel free to modify or enhance this configuration, feedback is welcome.


Plugin types
------------

The following plugins are available after installing the extension.


List of Posts by Date
^^^^^^^^^^^^^^^^^^^^^

Displays a list of blog posts ordered by date. All non-hidden, non-deleted and non-archived posts are shown in the list.

.. figure:: ../Images/Frontend/list.png
   :scale: 50%

   Frontend view of blog post list.

.. figure:: ../Images/Plugins/list.png

   Backend view of blog post list.


List by Tag
^^^^^^^^^^^^

Allows the users to show all posts tagged with a specific keyword.

.. image:: ../Images/Plugins/byTags.png


List by Category
^^^^^^^^^^^^^^^^

If you add this element and you have selected a category on the categories tab, it will show an overview of posts for
that category. If you have no categories selected, it will show an overview of categories.

.. image:: ../Images/Plugins/byCategory.png


List by Author
^^^^^^^^^^^^^^

Displays all posts belonging to the chosen author.

.. image:: ../Images/Plugins/byAuthor.png


List of related posts
^^^^^^^^^^^^^^^^^^^^^

Based on the categories and tags of the current post, it will show a list of related posts. This overview should only be
placed on a Blog detail page.

.. image:: ../Images/Plugins/relatedPosts.png


Archive
^^^^^^^

The archive plugin displays all posts categorized by year and month.

.. image:: ../Images/Plugins/archive.png


Other plugin types
^^^^^^^^^^^^^^^^^^

Additionally to the list plugin types there are several others meant to give you the maximum flexibility. If you are using the
templates included in the extension you won't need them as they represent parts you'd normally want to have at fixed positions
in your templates. For special circumstances we provide these plugins as standalone versions so you can use them in every
way you want:


Sidebar
"""""""

The sidebar contains links enabling the user to quickly navigate your blog. It shows an overview of recent posts and comments,
categories, tags and archive links.

.. figure:: ../Images/Frontend/sidebar.png
   :scale: 50%

   Sidebar of a blog


Latest posts
""""""""""""
This plugin is new. It allows to configure how many of the latest news shall be displayed in a list with the same format as the list of posts plugin.


Header and Footer
"""""""""""""""""
These two plugins are also new. They are meant to be used solely inside a post and if you apply these plugins in a different context, you will get an error message in the frontend. All meta data is now displayed with either one of the two plugins or through a combination of both.


Metadata
""""""""
This plugin is the old way of dealing with metadata and is currently deprecated. You are recommended to use Header and/or Footer to display meta data, like date, tags and category. The metadata plugin wil be removed in the upcoming version of the Blog extension.


Authors
"""""""
Displays post authors, like name, title, avatar, social links...


Comments / Comment Form
"""""""""""""""""""""""

Displays the comment form and comments to a post - be aware that commenting in general has to be globally enabled and the
respective post should have the commenting flag set.


Creating Categories and Tags
----------------------------

Categories are the default TYPO3 categories you probably already know.

Create a new category:

* Go to the list module
* Click on the page where you want to create the new category
* Click on the "new record" button on the top and choose category
* Enter a title for the category and choose a possible parent
* Click "Save"

Tags are blog specific records. Creating a new tag works in the same way as creating categories does:

* Go to list module
* Click on the page where you want to create the new tag
* Enter a title for the tag
* Click "Save"

Enable sharing
--------------
No implementation is provided by the blog extension itself. Of course you can still use an extension like the Shariff implementation for TYPO3 in your custom templates.


AvatarProvider
--------------
The default AvatarProvider is the GravatarProvider, this means the avatar of an author is received from gravatar.com. The extension provides also an ImageProvider for local stored images.

But you can also implement your own AvatarProvider:

1. Create a class which implements the AvatarProviderInterface.
2. Add your provider to the TCA field “avatar_provider” to make it selectable in the author record

**Note:** Since v10 the proxying of gravatar loading is used which means that TYPO3 downloads the gravatar, stores it on the filesystem and delivers the image locally from typo3temp. This is privacy related and useful if users didn't give their consent for fetching gravatars client side.
