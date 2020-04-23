---
title: Hacker Code UK
---

Hi and welcome to [HackerCode.UK](https://hackercode.uk).

The meaning of `hacker` in here, is to be related to the `good hacker` or `white hacker`.

> White hats are hackers who work to keep data safe from other hackers by finding system vulnerabilities that can be mitigated.
> White hats are usually employed by the target system's owner and are typically paid (sometimes quite well) for their work.
> Their work is not illegal because it is done with the system owner's consent.
> Source <https://en.wikipedia.org/wiki/Hacker#White_hat_hacker>

---

## Lastest posts

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      {{ post.excerpt }}
    </li>
  {% endfor %}
</ul>
