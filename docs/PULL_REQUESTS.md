# Pull requests

To add code to the different repositories the code is build in a feature branch and merged back to the branch ``develop``
using a pull request. No direct committing is allowed to the branches ``develop`` and ``main``.   
When a release is created the branch ``develop`` is merged to the branch ``main`` using a pull request.  
In some repositories the branch ``develop`` is skipped and not there. Feature branches are merged directly to the branch
``develop``

## Creating a pull request

When creating a pull request to merge a feature branch back we need to be aware that some information about the pull
request is used to generate the release notes. 

- **Title**: The title is added to the release notes, so give it a nice descriptive one.
- **Labels**: Use a label to add the pull request to correct section in the release notes.
  - ``enhancement``: The pull request adds as a new feature.
  - ``bug``: The pull request solves a bugfix.
  - ``tooling``: Change or update to tooling used to build project.

There is a special label  ``dependencies`` used by dependabot for updating dependencies.
These are grouped together in a separate section.  
If no label is added the pull request will be added to the section ``Other Changes`` at the bottom of the release notes.

The following labels cause the pull request to be ignored in the release notes:
- ``wontfix``
- ``duplicate``
- ``invalid``
