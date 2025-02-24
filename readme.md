### This site was built with Hugo using the Poison theme.

Helpful commands:

```shell
# build
hugo
# run
hugo server
# run including drafts
hugo server -D
# run without caching
hugo server --noHTTPCache

# create new content
hugo new
```

Github Branches:

```
Default: main
Prod: prod
```

### Handy Shortcodes

Tabs:

```md
{{< tabs tabTotal="2" >}}

{{% tab tabName="Resume" %}}
This is markdown content.
{{% /tab %}}

{{< tab tabName="About" >}}
{{< highlight text >}}
This is a code block.
{{< /highlight >}}
{{< /tab >}}

{{< /tabs >}}
```

Dropdown:

```md
{{< details summary="Where's daddy?" >}}
Suprise!
{{< /details >}}
```
