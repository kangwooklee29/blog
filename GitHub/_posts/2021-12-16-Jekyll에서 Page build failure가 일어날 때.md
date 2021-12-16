```html
The page build failed for the `main` branch with the following error:

The variable `...` on line ... in `...` was not properly closed with `...`. For more information, see https://docs.github.com/github/working-with-github-pages/troubleshooting-jekyll-build-errors-for-github-pages-sites#tag-not-properly-terminated.

For information on troubleshooting Jekyll see:

 https://docs.github.com/articles/troubleshooting-jekyll-builds

If you have any questions you can submit a request at https://support.github.com/contact?tags=dotcom-pages&repo_id=411364478&page_build_id=300906686
```
{% raw %}
가끔 깃헙에서 이런 이메일이 날아오면서 페이지 빌드가 안될 때가 있다. Jekyll은 페이지로 빌드할 때 Liquid를 사용해 페이지를 빌드하는 데 사용하는 소스파일 안의 Liquid 코드를 처리하는데, 만약 그 소스파일 안에 Liquid 코드가 제대로 문법대로 작성되지 않는다면 Page build failure가 발생한다. 

설사 그 파일 내에서 Liquid 코드를 쓸 의도가 없었다 하더라도 {{, }}, {%, %} 같은 Liquid template tag를 잘못 사용할 경우 이러한 에러가 일어날 수 있다. 이 경우 다음과 같은 방법으로 문제를 해결할 수 있다.

1. Liquid template tag를 {{"..."}} 태그의 따옴표 안에 쓴다.

2. Liquid template tag 앞뒤로 {% raw %}, {% endraw %} &#123;&#123;"&#123;% endraw %&#125;"&#125;&#125;를 쓴다.

3. Liquid template tag의 {, }를 쓸 자리에 &#38;#123;(='{'), &#38;#125;(='}')를 쓴다.
