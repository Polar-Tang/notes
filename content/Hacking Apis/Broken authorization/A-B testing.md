
A-B testing consinst in having mutliple account with different privileges, (an account wihtout login, a user free, a vip user) a compared each other for detecting patterns.
You want to test for:
- How the resourses are identified and how these are requested (GUID, ID, etc.)
- Swap your user A account token to the user B acount token to see if this way you could access the resources of the user B
The scale of this testing is small, but if you can access one user’s resources,
you could likely access all user resources of the same privilege level.