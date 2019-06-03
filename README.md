# gradle-mkdocs-example
This project provides a template for building documentation websites with [Gradle](https://www.gradle.org) and [MkDocs](https://www.mkdocs.org/).

## Prerequisites
This project requires that you have the following prerequisites installed on your system:

* [Python](https://www.python.org/downloads/)
* [Pip](https://pip.pypa.io/en/stable/installing/)

## Development
The website is built using markdown files processed by [MkDocs](https://www.mkdocs.org/).

### Internal Links
When the production site is built, all of the markdown pages (e.g. `foo.md`) become represented in the site as `http://docs.netifi.com/x.y.z/foo/[index.html]`. This has implications for how to correctly create internal links in the documentation pages. Links that work correctly when you view the site locally (using `./gradlew mkdocsServe`) may *not* work on the live site if you do not form them properly. Consult the following chart

| link format            | result |
| ---------------------- | ------ |
| `[link text](../page)` | **CORRECT.** |
| `[link text](./page)`  | **INCORRECT.** Will incorrectly link to the new page as though it were in the subdirectory formed by the current page. |
| `[link text](page)`    | **INCORRECT.** Will incorrectly link to the new page as though it were in the subdirectory formed by the current page. |
| `[link text](/page)`   | **INCORRECT.** Will incorrectly link to the new page as through it were at the root of the docs.netifi.com site without the `x.y.z` version number. |

If you add new subdirectories to the documentation site, this may become yet more complex.

The above does not apply to the main documentation site `index.md` file. In that file, use the simple `[link text](page)` format to refer to other pages on the site.

### Building the Site
Run the following command to build the website:

    ./gradlew clean mkdocsBuild
    
Under the `/build` directory you will now see a `mkdocs` folder containing the latest version of the site and a master
`index.html` file that redirects the user to the latest version of the documentation.

### Adding a New Page
Follow the steps below to add a new page to the site:

1. Add new markdown file under [/src/doc/docs](./src/doc/docs)

2. Update the `nav` element in [mkdocs.yml](./src/doc/mkdocs.yml) to add the page at your desired location in the site hierarchy.

### Running Site with Live Reload
During development it may be helpful to have the server continuously reload the site as changes are made so you can preview
your modifications. 

Run the following command to start the development server:

    ./gradlew mkdocsServe

The site will be available at: [http://localhost:8000](http://localhost:8000/)

## Release
Follow the steps below to release a new version of the site:

1. Update the `version` property in the root [build.gradle](./build.gradle) file to the current version.

2. Run the following command to build the website:

        ./gradlew clean buildRelease

3. Validate that there is a new folder with the version number as the name in the `/site` directory.

4. Validate that the `index.html` file in the `/site` directory redirects to the new version of the documentation.

5. Check-in / merge all new files to the `master` branch in GitHub. This will kick off an automated deployment process in [Netlify](https://www.netlify.com).

6. Once the Netlify deploy process has completed, validate that the website now points to the new version of the documentation.

## Additional Documentation

* [Material Theme](https://squidfunk.github.io/mkdocs-material/)
* [Mkdocs](https://www.mkdocs.org/user-guide/writing-your-docs/)