# Publishing workflows

GraphCMS has a simple, built in publishing workflow that allows you to control which content will be delivered by your API. This lets you create new content and only deliver it after it was reviewed.

!!! warning
    Currently, unpublished entries are also delivered from the API. In the future we won´t serve them via the API. For now, you can add a filter to your queries to ignore unpublished entries.

    Example: `{ allPosts(filter: {isPublished: true}) { title body }}`

To `publish` an entry you need to have the required [permissions](/guides/Adding_members_to_team/). In GraphCMS, all roles except the `CONTRIBUTOR` can publish existing entries. This can be done within the entry form.

To publish an existing entry you just have to click the `PUBLISHED` toggle at the top of the page, as shown in the image below. To unpublish an entry simply click the toggle again.

![Screenshot](../img/guides/publish_entry.png)

Users with the role `CONTRIBUTOR` are able to view published entries, but not edit or delete them. Also, they don´t see the `PUBLISHED` toggle, since they are not allowed to publish entries.

When a contributor opens a published entry, all form inputs and the save button of the form are disabled. An additional info message is shown to notify the user why the inputs are disabled (see image below).

![Screenshot](../img/guides/disabled_published_entry.png)
