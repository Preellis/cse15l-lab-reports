# Lab Report 4 Week 8
---
## Github Repos Used
For this lab report I'll be testing [the version of markdown parse we reviewed](https://github.com/21KennethTran/markdown-parser) and [the version we shared](https://github.com/leahkuruvila/markdown-parser).

## Snippets
The following snippets were placed into files named snippet1.md, snippet2.md, and snippet3.md in the folder created after cloning each directory.

Snippet 1:
```
`[a link`](url.com)

[another link](`google.com)`

[`cod[e`](google.com)

[`code]`](ucsd.edu)
```
Snippet 2:
```
[a [nested link](a.com)](b.com)

[a nested parenthesized url](a.com(()))

[some escaped \[ brackets \]](example.com)
```
Snippet 3
```
[this title text is really long and takes up more than 
one line

and has some line breaks](
    https://www.twitter.com
)

[this title text is really long and takes up more than 
one line](
https://sites.google.com/eng.ucsd.edu/cse-15l-spring-2022/schedule
)


[this link doesn't have a closing parenthesis](github.com

And there's still some more text after that.

[this link doesn't have a closing parenthesis for a while](https://cse.ucsd.edu/



)

And then there's more text
```

## Snippet 1
All tests expected output determined by CommonMark.

The following code was added to MarkdownParseTest.java in both repositories (note that the expected output is the list assigned to the expected variable).
```
@Test
public void testSnippet1() throws IOException {
    Path fileName = Path.of("snippet1.md");
    String content = Files.readString(fileName);
    List<String> links = MarkdownParse.getLinks(content);
    List<String> expected = List.of("%60google.com", "google.com", 
        "ucsd.edu");

    assertEquals(expected, links);
}
```

For the version we reviewed, the below was the associated JUnit output with failure:
```
1) testSnippet1(MarkdownParseTest)
java.lang.AssertionError: expected:<[%60google.com, google.com, ucsd.edu]> but was:<[]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet1(MarkdownParseTest.java:112)
```
For our implmentation, the below was the associated JUnit output with failure:
```
1) testSnippet1(MarkdownParseTest)
java.lang.AssertionError: expected:<[%60google.com, google.com, ucsd.edu]> but was:<[]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet1(MarkdownParseTest.java:112)
```

I think that the issue is fixable by a somewhat small code change. In order to solve the backticks issue, before each check for brackets, we would have to check for a backtick and then ignore all text until reaching the next backtick (note that we also have to continue to the next loop and update current index if we run into a double `\n`), and we would have to replace backticks inside links with their url shortcodes.
## Snippet 2
All tests expected output determined by CommonMark.

The following code was added to MarkdownParseTest.java in both repositories (note that the expected output is the list assigned to the expected variable).
```
    @Test
    public void testSnippet2() throws IOException {
        Path fileName = Path.of("snippet2.md");
        String content = Files.readString(fileName);
        List<String> links = MarkdownParse.getLinks(content);
        List<String> expected = List.of("a.com","a.com(())","example.com");

        assertEquals(expected, links);
    }
```
For the version we reviewed, the below was the associated JUnit output with failure:
```
2) testSnippet2(MarkdownParseTest)
java.lang.AssertionError: expected:<[a.com, a.com(()), example.com]> but was:<[]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet2(MarkdownParseTest.java:121)
```

For our implmentation, the below was the associated JUnit output with failure:
```
2) testSnippet2(MarkdownParseTest)
java.lang.AssertionError: expected:<[a.com, a.com(()), example.com]> but was:<[]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet2(MarkdownParseTest.java:121)
```

I think in order to fix all related cases with nested brackets/parens and escaped brackets would require a more significant change. This is because we have to not only check that brackets match up inside potential link constucts but we also have to check if there is an entire valid link inside the brackets or parens. I think that this would require either breaking out code into helper methods, complicated case checking, or switching to a different parsing method like using a stack or other data structure.
## Snippet 3
All tests expected output determined by CommonMark.

The following code was added to MarkdownParseTest.java in both repositories (note that the expected output is the list assigned to the expected variable).
```
    @Test
    public void testSnippet3() throws IOException {
        Path fileName = Path.of("snippet3.md");
        String content = Files.readString(fileName);
        List<String> links = MarkdownParse.getLinks(content);
        List<String> expected = List.of("https://sites.google.com/eng.ucsd.edu/cse-15l-spring-2022/schedule");

        assertEquals(expected, links);
    }
```
For the version we reviewed, the below was the associated JUnit output with failure:
```
3) testSnippet3(MarkdownParseTest)
java.lang.AssertionError: expected:<[https://sites.google.com/eng.ucsd.edu/cse-15l-spring-2022/schedule]> but was:<[
    https://www.twitter.com
,
https://sites.google.com/eng.ucsd.edu/cse-15l-spring-2022/schedule
, github.com

And there's still some more text after that.

[this link doesn't have a closing parenthesis for a while](https://cse.ucsd.edu/



]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet3(MarkdownParseTest.java:130)
```
For our implmentation, the below was the associated JUnit output with failure:
```
3) testSnippet3(MarkdownParseTest)
java.lang.AssertionError: expected:<[https://sites.google.com/eng.ucsd.edu/cse-15l-spring-2022/schedule]> but was:<[
    https://www.twitter.com
,
https://sites.google.com/eng.ucsd.edu/cse-15l-spring-2022/schedule
, github.com

And there's still some more text after that.

[this link doesn't have a closing parenthesis for a while](https://cse.ucsd.edu/



]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet3(MarkdownParseTest.java:130)
```

I think the fix for these cases are fairly simple. We can just add an extra check before looking for the next bracket/parenthesis if there is a double `\n` in between and then `continue` to the next part of the main loop after updating the current index to after the line break.