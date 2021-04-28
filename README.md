# Drupal (8.x) Best Practices

> This is a short, opinion based, **short list** with common best practices for developing a Drupal (8.x) website.

### 1. Parameters to consider
- **Technical specification document**.
- A **brief of the project scope**.
- **Old project** (if exists).
- Time-sheet and **delivery deadline**.
- **Multilingual** content.
- If only anonymous users access the site (no login options for anonymous).
- How many different Authors will add Content.
- Is there needed a **data migration**?
- Are there any **accessibility** valid code (eg [WCAG](https://www.w3.org/TR/WCAG21/)) requirements?
- **Integration** with 3rd parties (payments, authorization, REST services etc).
- Educate your client to phrase requirements.
- Do not make assumptions: know what you have to do or don’t do it.
- Plan the implementation, a good preparation is half the work done.
- Keep it simple: any new feature must be tested and can introduce bugs.

---

### 2. Site building

#### 2.1 Nodes
- Entity and bundle name should use only singular name (Correct:  content type "page". Incorrect: content type "pages").
- Don't use nodes to create dynamic blocks (eg a block slideshow). Create a custom entity instead or better a custom block type.
- Create a new Node bundle only when this bundle needs to have different display options, different functionality, too many different fields and needs specific management.
- Less bundles is better for the database and performance but needs more manipulation.
- Don't create new revisions by default except if the Node pages you create need to have workflow statues and they are really important (eg a simple news page on a 10k News website may not have revisions but the main product landing page could have).
- Display view modes should be generic for all Content types and not specific (eg event_full).
- Machine names of Content types and fields should disallow name conflicts (eg nameone and nameoneplus may cause issues).
- Content types should follow this pattern for the machine name: [machinename]. That means you should use only letters and no special character or space.
- Avoid very generic machine names or names that have been used already on the site even for another type of functionality (Views, Content types, Blocks, Plugins etc).

#### 2.2 Blocks
- Custom Blocks should follow this pattern for the machine name: [machinename]. That means you should use only letters and no special character or space.
- Create custom Block Types using code.
- Treat machine_name, fields, view modes etc like any other Content type (see above).
- On the machine_name do not add the block prefix (Drupal will add this anyway before the machine_name).
- On the machine_name add a basic, generic name that will describe the block functionality. Eg banner, list,  header etc.
- On the machine_name do not add any region/theme/placement etc related info.

#### 2.3 Taxonomy
- Entity and bundle name should use only singular name (Correct: vocabulary "author". Incorrect: vocabulary "authors").
- Use taxonomy terms only when you need to show pages of categorized content.
- Don't use taxonomy terms just to filter content. Instead use list type fields.
- When the taxonomies need to have authorization, extra fields and different display types investigate using a node type entity reference.

#### 2.4 Other content entities
- For other content entities like paragraphs, comments, media,  etc the rules are the same as for Nodes.
- Be careful with the translations especially for paragraphs because paragraphs use revisions by default.

#### 2.5 Fields
- Common fields should be named as generic as possible, but at the same time very clear (Correct: "field_body, field_summary, field_author". Incorrect: "field_article_body, field_b")
- Specific fields should have entity name in the name, separated by underscore (Correct: "field_author_name, field_author_mail, field_author_address". Incorrect: "field_name, field_authorname").
- Fields machine name should follow this pattern for the machine name:  `field_[content_type_machine_name]_[short_name]`.
- Re-use fields (shared fields) only when you need to create a reference using this field or the field does not change at all between the shared entities (eg field created).
- Reusable fields can have a more generic machine name pattern (eg field_shared_created or field_common_created etc).
- All fields require a Description to inform the user about their need.
- Image fields file directory should not use the default structure `[date:custom:Y]-[date:custom:m]` but a custom one meaningful name (eg `banners`). Unify image fields of the same type under the same folders.
- Remove `gif` from allowed file extensions for image fields except if there are special requirements.
- Consider using a fixed number of letters for the prefixes everywhere eg in a 3 letter prefix pattern there whould be `field_srd_` for shared fields, `field_art_` for Article node type only fields etc.

#### 2.6 (Drupal) Views
- Views should follow this pattern for the machine name: [machinename]. That means you should use only letters and no special character or space.
- Remove the suffix `_1` from the default machine name that Views create for a new Views display (eg "page_1" should be "page").
- It is required to give a meaningful (Administrative) name, description and tags to the Views. Do not leave the default values.
- Create one Views per Views Display except if it is a requirement. For example a Block Views and a Page Views for the same Content type should exist on different Views.
- Always use `Format > Show: Content` for the views row display and not fields. This way the styling will be tied with the Content types View Modes. If there is no available content type View Mode create one.
- Do not add custom CSS classes on the whole Views.
- When packaging with Drupal Features add the Views with the associated Entity type (eg Blog) except if there is a special use case of the Views.
- Always override default system views if they are to be used on the project.
- Do not use Ajax by default for a View.
- Always provide a simple text for "No results behavior".
- Always use `Access: Permission | ...` for the Views access. Do not use Roles.
- Add useful Administrative comments for each Views.
- Always provide a Title for the Views.
- Disable all unused Views.
- If you clone a View be careful to satisfy the above rules.

#### 2.7 Forms
- Use [webform](https://www.drupal.org/project/webform) module to create custom forms.
- Use **core Contact form** only if there are no special requirements (eg one only contact form, no need to keep submissions, few filds only etc).

#### 2.8 Text formats and editors
- Try to use only 1 HTML allowed editor format. Multiple HTML allowed formats will have issues with multiple authors with different editing permissions.
- Administrators should not use any extra format (full_html etc) that will not be available for Authors since all the Content must be editable by the portal Managers/Authors etc.
- Use CKEditor for HTML allowed format.
- All users should not have the option to change between plain_text and html.
- Insecure piece of content (eg PHP) should be added programmatically from code and not on the wysiwyg textarea.
- The WYSIWYG editor buttons should be the same as the HTML tags allowed. nothing more, nothing less.
- WYSIWYG should inherit exactly the frontend Theme CSS/JS.
- WYSIWYG should not allow the user to add styles that do not have a style.
- WYSIWYG should not allow the user to create functionality inside the Editor such as dropdown menus or effects.
- For each allowed option on the Drupal text Filter WYSIWYG should provide the relative button. Eg if the user is able to add a `<blockquote>` she should see a button on the Editor to add a blockquote.
- Custom Ckeditor templates should be created (using JS) on a custom module.
- For accessibility valid code there should be forced elements on CKEditor (eg required alt attribute for images).
- WYSIWYG should provide basic information about using the Editor as also as a more analytic page for Admins.
- Too many options on the WYSIWYG may cause problems. Protect the user from "breaking" the site design/code etc.
- If you allow the "image" button for CKEditor you normally need to install an image browser like [imce](https://www.drupal.org/project/imce).
- Be careful with the CKEditor `inline-images`. Normally you should not allow users to add images on CKEditor but use a specific image field to do so.
- If you allow the "link" button for CKEditor you normally need to install additional modules such as [linkit](https://www.drupal.org/project/linkit), [editor_advanced_link](https://www.drupal.org/project/editor_advanced_link), [pathologic](https://www.drupal.org/project/pathologic) etc.

#### 2.9 Menus & navigation
- If using a pathauto pattern for path aliases consider using structured paths where each parent has a page.
- Avoid using non English characters for path aliases.
- For placeholder paths (that go nowhere) use the `<nolink>` as path value.
- Menus should always be menus. Non menu blocks used as menus have accessibility issues and missing valuable details for the browser (eg the `is-active` class).

#### 2.10 Users, roles & permissions
- Use an "Administrator" role only when you need to add more than 1 Administrators.
- Split roles by Persona (not by functionality).
- Do not allow authors access pages and options that they have nothing to do (hide empty admin pages).
- Don't allow multiple real persons to share the same Drupal Account. Consider creating 1 account per person.
- Give short one word machine names to user Roles (eg prefer sitemanager instead of site_manager).
- Do not add any permissions for the core user role "Authenticated User" (authenticated) except if there are no other member roles.
- Short roles by permissions quantity. For example Anonymous shoulld be on top and Administrators should be below any other roles.
- Every custom module should define its own permissions. Never reuse an existing permission!
- If there is separate functionality on a custom module create all the necessary permissions (eg permission to "Access the display" and to "Administer the module" etc).
- All the permissions titles and descriptions should start with a common used verb such as: "View …", "Administer …", "Delete …" etc.
- Use module [masquerade](https://www.drupal.org/project/masquerade) to test other members permissions.
- Create **custom admin dashboards**, do not let authors use the default Drupal admin dashboard/backend.

---

### 3. Theming, templates
- Follow the [Atomic design](http://patternlab.io/about.html) philosophy for the CSS as also as for the design and html.
- For multiple themers team or large projects prefer working with **twig templates** only (eg adding html classe for each view mode) so no theme settings exist on database.
- For 1 themer team you can use the field UI to move fields around (so theming exists on database).
- Avoid using modules **panels, panelizer, panels_everywhere** and of this family.
- Use preprocess functions on `.theme` file to add custom html classes or other attributes, twig variables etc.
- Don't forget to create templates for special pages (404, 403, maintenance, login page etc).
- For large projects with many templates investigate using modules [components](https://www.drupal.org/project/components), [ui_patterns](https://www.drupal.org/project/ui_patterns) and family, [storybook](https://storybook.js.org/), [pattern_library](https://www.drupal.org/project/pattern_library) as also as [Patternlab library](https://drupal-pattern-lab.github.io).
- Try to use only core theme ([Classy](http://cgit.drupalcode.org/drupal/tree/core/themes/classy)) as base theme.
- If you want to use a contrib base theme (eg bootstrap, omega etc) it is better to clone and override it and use no base theme.
- It is a good practice to have a project styleguide.
- For complicated content structure requirements investigate using module [paragraphs](https://www.drupal.org/project/paragraphs).
- Consider using the most common breakpoints for responsive design and use the same breakpoints for Drupal breakpoints also (340px, 768px, 1024px, 1600px).
- Use SASS/SCSS.
- Support IE11 and above browsers and follow the [Drupal project browser support](https://www.drupal.org/docs/8/system-requirements/browser-requirements). We are not styling with that in mind but will deal with it when finished.
- Override only the necessary CSS coming from each module. Do not copy and paste all the CSS of a module to the project Theme.
- Be careful when overriding CSS/JS as also as html structure especially if it is coming from Core. This may break core built-in functionality.
- For large scale projects investigate using a "utility-first/functional" CSS framework like [tailwind](https://tailwindcss.com), [tachyons](http://tachyons.io) etc.
- For the Admin pages use the default Core admin theme (Seven) and simply attach your CSS/JS files to add some functionality. This will happen on a custom module or a subtheme.
- If you want to override a twig template get only from the source module that provides that. Eg if you want to override the `html.html.twig` file copy that from the system module.
- Under the Theme folder create new subfolders to group files only if there is a significant amount of them and it makes sense to separate theme. Eg split the twig files to templates/system, templates/block etc using the pattern templates/[module] if there are 3 files of the system and 2 files of the block module.
- Avoid using the path alias (eg related classes) of a webpage to style the page. Path aliases are considered unstable and could change frequently.
- Do not use non semantic CSS classes. Saying that we should not use classes such as "clearfix, container, full-width" etc if these classes are used to provide some CSS styling. Instead use SASS mixins such as `@full-width {width: 100%}`.
- Avoid theming with elements except from the basic ones (p, h1 etc). Use CSS classes instead.
- Use special prefixes for css classes that are depended to their functionality or usage (eg `twig-` for twig manual classes, `js-` for classes that trigger a js action etc).
- If you need to use a CSS framework try to pick up only the required parts of the framework and not the whole framework.
- Always comment @mixins, @function.
- Always split large scss files if they contain too many lines. Split them by usage (eg the variables related to color, the variables related to typography etc). For Drupal especially, we can group scss files by entity (eg "block/\*", "node/\*", "taxonomy/\*" etc).
- Useful plugins to use for theme development are [autoprefixer](https://autoprefixer.github.io), [breakpoint-sass](http://breakpoint-sass.com), [ModularScale](https://www.modularscale.com), [browsersync](https://browsersync.io), [sass sourcemaps](http://thesassway.com/intermediate/using-source-maps-with-sass).

---

### 4. Development

#### 4.1 Documentation
- Create a README file written for humans where you store all the development steps.

#### 4.2 Coding
- Private functions should never be called from outside of the module in which they are declared.
- Private functions should be declared at the bottom of a file.
- Php Classes and Functions MUST be documented with "docblocks".
- All custom specific modules MUST be prefixed with `projectmachinename_`.

#### 4.3 Drupal scaffolding
- Use only 1 custom `settings.php` that includes environment specific additional settings files. Track the settings.php file on git but not the additional settings files.
- Try to keep important Drupal settings on the settings.php additional files (eg enabled development modules, caching options, php ini settings etc) and not on the database.
- Settings.php and additional files should work on a CI system and they should be "platform agnostic".
- Use composer for dependencies and track only composer.json and composer.lock files on git.
- Do to not track composer downloaded files on git (eg module/contrib, themes/contrib, core, vendor folders etc). Track them only if you have no options to run `composer install` on the online servers.
- Patches should be added with composer using [cweagans/composer-patches](https://github.com/cweagans/composer-patches).
- Keep the `config` folder out of the web files folder but track it on git.
- Keep the `vendor` folder out of the web files folder.
- Scaffolding code should exist on the project root (eg ansible, bash, dockerfile etc) either to create the environment on a CI or a VM.
- Use [Drupal Features](https://www.drupal.org/project/features) to organize your config into modules only when you need to export this to reusable modules.
- Use `drush config-import/config-export` (or additional drush options like `drush csex/csim`) to manage config.
- Less modules is better.
- Try to use only core modules and add contributed modules only if required.

#### 4.4 Infrastructure
- Don't rise the recommended php memory_limit when there are relevant issues. Try to figure out what causes the memory_limit timeout.
- Run cron externally (disable core cron settings).
- Use a custom SMTP server to send emails.

#### 4.5 Third party libraries
- Before adding an external library/dependency check if it is already available [on Core](http://cgit.drupalcode.org/drupal/tree/core/core.libraries.yml).
- Prefer tiny and specific libraries.
- Check the library popularity on its official repository. Avoid libraries with no traffic at all.
- If multiple release sources (eg packagist, apt-get etc) are available for the library prefer GitHub which is most of the times the development source.
- Always use a release of a library (if available) and not a generic branch.
- If you care about performance try to download the libraries locally instead of using a CDN source.

#### 4.6 Modules to use for development
- [stage_file_proxy](https://www.drupal.org/project/stage_file_proxy)
- [devel](https://www.drupal.org/project/devel)
- [masquerade](https://www.drupal.org/project/masquerade)
- [reroute_email](https://www.drupal.org/project/reroute_email)
- [simplei](https://www.drupal.org/project/simplei)
- [structure_sync](https://www.drupal.org/project/structure_sync)
- [config_split](https://www.drupal.org/project/config_split)
- [backup_migrate](https://www.drupal.org/project/backup_migrate)

---

### License
[MIT](LICENSE)
