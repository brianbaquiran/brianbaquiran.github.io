.. title: GitLab Weirdness
.. slug: gitlab-weirdness
.. date: 2016-09-28 15:35:40 UTC+08:00
.. tags: tips, git
.. category:
.. link:
.. description:
.. type: text


If you're using GitLab.com for hosting your repositories, you may have encountered
a strange problem wherein your newly-created repository's dashboard doesn't update.

.. thumbnail:: /images/gitlab-weirdness.png

That is, when you `git push` your changes to the repository, the interface still
looks like a newly-created repository, and neither your files nor your commits
are visible in the web UI. This is weird because the remote repository works in
all other respects. You can push code up to it, clone it, etc. You just can't see
it on the GitLab website.

I've seen this happen a couple of times, and so far I've found that the quick fix is to
run Housekeeping on the repository from the Edit Project page.

.. thumbnail:: /images/gitlab-housekeeping.png

Housekeeping can take a couple of minutes but most of the time it works and you
can see your repository's files and commit history after running it. If it
doesn't work, you have to delete the repository in GitLab and re-create it,
pushing your code up again.
