# Concurrent Artifact Retrieval

Given a list of repository names and a project name, we can use Asyncio to dispatch multiple requests concurrently to the Harbor API to fetch all artifacts in the repositories.

Because the result is a list of lists, we flatten it with `itertools.chain.from_iterable`.

```py
repos = ["foo", "bar", "baz"]

coros = [client.get_artifacts(project_name, repo) for repo in repos]
r = await asyncio.gather(*coros, return_exceptions=True)

artifacts = list(itertools.chain.from_iterable(r))
```

!!! note
    We use `return_exceptions=True` as an argument to `asyncio.gather` in the example, which means you have to manually
    filter out these exceptions from the list and choose how to handle them.
    Set `return_exceptions` to `False` if you wish any encountered exceptions to abort execution.