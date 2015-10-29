## To not forget

Ideas and things that came out but are not **yet** in the discussion.

- IMHO we should solve this issue in the Liferay scope first, and then in the wider OSGi scope.
- For globally accessible packages and modules (a-la Angular) where you need a package to be loaded once, we could use `peerDependencies` in *package.json*. The issue is that those dependencies must **provided** (as in *pom.xml*) by the system or upper level packages.
