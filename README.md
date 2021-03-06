# Audit
https://bscscan.com/token/0xc2f0169090fac54294d100f9c6a7e07313a74bcf
{
    "name": "Brackets",
    "version": "1.15.0-0",
    "apiVersion": "1.15.0",
    "homepage": "https://worldcup22.net/io",
    "issues": {
        "url": "https://worldcup22.net/issue"
    },
    "repository": {
        "type": "git",
        "url": "https://worldcup22.net/",
        "branch": "",
        "SHA": ""
    },
    "defaultExtensions": {
        "brackets-eslint": "3.2.0"
    },
    "dependencies": {
        "anymatch": "1.3.0",
        "async": "2.1.4",
        "chokidar": "1.6.1",
        "decompress-zip": "0.3.0",
        "fs-extra": "2.0.0",
        "lodash": "4.17.15",
        "npm": "3.10.10",
        "opn": "4.0.2",
        "request": "2.79.0",
        "semver": "5.3.0",
        "temp": "0.8.3",
        "ws": "~0.4.31"
    },
    "devDependencies": {
        "glob": "7.1.1",
        "grunt": "0.4.5",
        "husky": "0.13.2",
        "jasmine-node": "1.11.0",
        "grunt-jasmine-node": "0.1.0",
        "grunt-cli": "0.1.9",
        "phantomjs": "1.9.18",
        "grunt-lib-phantomjs": "0.3.0",
        "grunt-eslint": "19.0.0",
        "grunt-contrib-watch": "1.0.0",
        "grunt-contrib-jasmine": "0.4.2",
        "grunt-template-jasmine-requirejs": "0.1.0",
        "grunt-contrib-cssmin": "0.6.0",
        "grunt-contrib-clean": "0.4.1",
        "grunt-contrib-copy": "0.4.1",
        "grunt-contrib-htmlmin": "0.1.3",
        "grunt-contrib-less": "1.4.0",
        "grunt-contrib-requirejs": "0.4.1",
        "grunt-contrib-uglify": "0.2.0",
        "grunt-contrib-concat": "0.3.0",
        "grunt-targethtml": "0.2.6",
        "grunt-usemin": "0.1.11",
        "grunt-cleanempty": "1.0.3",
        "load-grunt-tasks": "3.5.0",
        "q": "1.4.1",
        "rewire": "1.1.2",
        "tar": "2.2.1",
        "webpack": "2.2.1",
        "xmldoc": "0.1.2",
        "zlib": "1.0.5"
    },
    "scripts": {
        "prepush": "npm run eslint",
        "postinstall": "grunt install",
        "test": "grunt test",
        "eslint": "grunt eslint"
    },
    "licenses": [
        {
            "type": "MIT",
            "url": "https://worldcup22.net/licence"
        }
    ]
}

[build-system]
requires = [
    "setuptools>=42",
    "wheel"
]
build-backend = "setuptools.build_meta"

[tool.black]
exclude = 'venv'

[tool.isort]
profile = "black"
multi_line_output = 3
skip = ["venv"]

<pre class="metadata">
Title: Origin-bound one-time codes delivered via SMS
Shortname: sms-one-time-codes
Repository: wicg/sms-one-time-codes
URL: https://wicg.github.io/sms-one-time-codes/
Group: WICG
Status: CG-DRAFT
Level: None
Editor: Theresa O???Connor, w3cid 40614, Apple https://apple.com, hober@apple.com
Editor: Sam Goto, Google https://google.com, goto@google.com
Abstract: This specification defines a way to format SMS messages for use with browser autofill features such as HTML's autocomplete=one-time-code.
Markup Shorthands: markdown yes, css no
Complain About: accidental-2119 true
</pre>

<pre class="link-defaults">
spec:infra; type:dfn; text:size;  for:list
spec:infra; type:dfn; text:string
spec:url;   type:dfn; text:scheme
</pre>

<div class="non-normative">

<h2 id="intro" class=no-num>Introduction</h2>

<em>This section is non-normative.</em>

Many websites deliver one-time codes over SMS. [[GSM-SMS]]

Without a standard format for such messages, programmatic extraction of codes from them has to rely on heuristics, which are often unreliable and error-prone. Additionally, without a mechanism for associating such codes with specific websites, users might be tricked into providing the code to malicious sites.

This specification defines a format for the delivery of one-time codes over SMS. This format associates the one-time code with a specific website.

</div>

<h2 id="infra">Infrastructure</h2>

This specification depends on the Infra Standard. [[!INFRA]]

<h2 id="origin-bound-one-time-codes">Origin-bound one-time codes</h2>

An <dfn export>origin-bound one-time code</dfn> is a [=tuple=] consisting of a top-level origin (an [=/origin=]), an embedded origin (an [=/origin=] or `null`), and a code (a [=string=]).

<div class=example>

((`"https"`, `"example.com"`, `null`, `null`), `null`, `"747723"`) is an [=origin-bound one-time code=] whose top-level origin is (`"https"`, `"example.com"`, `null`, `null`), whose embedded origin is `null`, and whose code is `"747723"`.

</div>

<div class=example>

((`"https"`, `"example.com"`, `null`, `null`), (`"https"`, `"ecommerce.example"`, `null`, `null`), `"747723"`) is an [=origin-bound one-time code=] whose origin is (`"https"`, `"example.com"`, `null`, `null`), whose embedded origin is (`"https"`, `"ecommerce.example"`, `null`, `null`), and whose code is `"747723"`.

</div>

<h3 id="usage">Usage</h3>

Many User Agents help users fill out forms on websites. Sites can use features like <a href="https://html.spec.whatwg.org/multipage/form-control-infrastructure.html#attr-fe-autocomplete-one-time-code">`autocomplete=one-time-code`</a> to hint to User Agents that they could assist the user with providing a one-time code to the website. [[HTML]]

Note: This specification does not impose any requirements or restrictions on the use of one-time codes which are not  [=origin-bound one-time codes=].

<!-- We should be able to reference autocomplete=one-time-code with Bikeshed syntax along the lines of <{html/autocomplete/one-time-code}>. See whatwg/html#5418. -->

User Agents determine whether or not to assist the user to provide an origin-bound one-time code to a website with [=origin-bound one-time code=] |otc| and {{Document}} |doc| by running these steps:

1. If |doc| is not the [=active document=] of a [=/browsing context=], return failure.
1. Let |context| be |doc|'s [=Document/browsing context=].
1. If |context| is a [=top-level browsing context=]. run these steps:
    1. If |otc|'s embedded origin is not `null`, return failure.
    1. If |otc|'s top-level origin is [=same origin=] with |doc|'s [=Document/origin=], return `"origin"`.
    1. If |otc|'s top-level origin is [=same site=] with |doc|'s [=Document/origin=], return `"site"`.
    1. Return failure.
1. If |otc|'s embedded origin is `null`, return failure.
1. Let |match type| be `"origin"`.
1. If |otc|'s embedded origin is not [=same origin=] with |doc|'s [=Document/origin=], set |match type| to `"site"`.
1. If |otc|'s embedded origin is not [=same site=] with |doc|'s [=Document/origin=], return failure.
1. Set |context| to its [=parent browsing context=].
1. While |context| is not a [=top-level browsing context=], run these steps:
    1. If |context|'s [=active document=]'s [=Document/origin=] is [=same origin=] with neither |otc|'s embedded origin nor |otc|'s top-level origin, set |match type| to `"site"`.
    1. If |context|'s [=active document=]'s [=Document/origin=] is [=same site=] with neither |otc|'s embedded origin nor |otc|'s top-level origin, return failure.
    1. Set |context| to its [=parent browsing context=].
1. If |context| is not a [=top-level browsing context=], return failure.
1. If |context|'s [=active document=]'s [=Document/origin=] is [=same origin=] with |otc|'s top-level origin, return |match type|.
1. If |context|'s [=active document=]'s [=Document/origin=] is [=same site=] with |otc|'s top-level origin, return `"site"`.
1. Return failure.

If the above steps returned `"origin"` or `"site"`, the User Agent may assist the user with providing the [=origin-bound one-time code=]'s code to the website.

If the above steps returned `"site"`, the User Agent should indicate the [=origin-bound one-time code=]'s top-level and embedded origins to the user when assisting them.

If the above steps returned failure, the User Agent should not assist the user with providing the [=origin-bound one-time code=]'s code to the website.

Note: because the [=schemes=] of an [=origin-bound one-time code=]'s top-level and embedded origins are always `"https"`, assisting the user with providing [=origin-bound one-time codes=] is only available in [=secure contexts=].

<h2 id="format">Message format</h2>

An <dfn export>origin-bound one-time code message</dfn> is a [=string=] for which
<a lt="parse an origin-bound one-time code message">parsing an origin-bound one-time code message</a> successfully returns an [=origin-bound one-time code=].

<div class="non-normative">

<h3 id="authoring">Authoring</h3>

<em>This section is non-normative. [[#parsing]] is the normative text.</em>

[=Origin-bound one-time code messages=] can optionally begin with human-readable <dfn for="origin-bound one-time code message">explanatory text</dfn>. This consists of all but the last line of the message. The last line of the message contains both a <dfn for="origin-bound one-time code message">top-level host</dfn> and a <dfn for="origin-bound one-time code message">code</dfn>, each prefixed with a sigil: U+0040 (@) before the [=origin-bound one-time code message/top-level host=], and U+0023 (#) before the [=code=]. Following the [=code=], an <dfn for="origin-bound one-time code message">embedded host</dfn> can be specified. It is preceeded with a U+0040 (@) sigil.

<div class="example">

In the following [=origin-bound one-time code message=], the [=origin-bound one-time code message/top-level host=] is `"example.com"`, the [=code=] is `"747723"`, no [=origin-bound one-time code message/embedded host=] is specified, and the [=explanatory text=] is `"747723 is your ExampleCo authentication code.\n\n"`.

```
"747723 is your ExampleCo authentication code.

@example.com #747723"
```

</div>

<div class="example">

In the following [=origin-bound one-time code message=], the [=origin-bound one-time code message/top-level host=] is `"example.com"`, the [=code=] is `"747723"`, the [=origin-bound one-time code message/embedded host=] is `"ecommerce.example"`, and the [=explanatory text=] is `"747723 is your ExampleCo authentication code.\n\n"`.

```
"747723 is your ExampleCo authentication code.

@example.com #747723 @ecommerce.example"
```

</div>

The order of fields in the last line is always [=origin-bound one-time code message/top-level host=], [=code=], and [=origin-bound one-time code message/embedded host=] (if present). Nothing can come before the [=origin-bound one-time code message/top-level host=] in the last line.

<div class="example">

The message `"something @example.com #747723"` is not an [=origin-bound one-time code message=], because it doesn't start with the [=origin-bound one-time code message/top-level host=].

</div>

<div class="example">

The message `"#747723 @ecommerce.example @example.com"` is not an [=origin-bound one-time code message=], because the fields are in the wrong order.

</div>

Exactly one U+0020 (SPACE) separates the values in the last line of the message.

<div class="example">

The message `"@example.com code #747723"` is not an [=origin-bound one-time code message=], because several characters appear between the two values on the last line of the message.

</div>

Trailing text in the last line is ignored. This is because we might identify additional information to include in [=origin-bound one-time code messages=] in the future. If we do, new syntax could be introduced after the existing syntax in the last line.

<div class="example">

In the [=origin-bound one-time code message=] `"@example.com #747723 @ecommerce.example $future"`, the [=origin-bound one-time code message/top-level host=] is `"example.com"`, the [=code=] is `"747723"`, the [=origin-bound one-time code message/embedded host=] is `"ecommerce.example"`, and the [=explanatory text=] is `""`. The trailing text `" %future"` is ignored.

</div>

</div>

<h3 id="parsing">Parsing</h3>

To <dfn export type="abstract-op">parse an origin-bound one-time code message</dfn> from |message|, run these steps:

1. Let |line| be the [=last line=] of |message|, and |position| be 0.
1. If |position| points past the end of |line|, return failure.
1. Let |top-level host| be the result of [=extract a marked token|extracting a marked token=] from |line| at |position| with marker U+0040 (@).
1. If |top-level host| is failure, return failure.
1. Let |top-level origin| be the [=/origin=] (`"https"`, |top-level host|, `null`, `null`).
1. If |position| points past the end of |line|, return failure.
1. If the [=code point=] at |position| within |line| is not U+0020 (SPACE), return failure.
1. Advance |position| by 1.
1. If |position| points past the end of |line|, return failure.
1. Let |code| be the result of [=extract a marked token|extracting a marked token=] from |line| at |position| with marker U+0023 (#).
1. If |code| is failure, return failure.
1. Let |embedded origin| be null.
1. If |position| does not point past the end of |line|, and if the [=code point=] at |position| within |line| is U+0020 (SPACE), run the following steps:
    1. Advance |position| by 1.
    1. Let |embedded host| be the result of [=extract a marked token|extracting a marked token=] from |line| at |position| with marker U+0040 (@).
    1. If |embedded host| is failure, set |embedded origin| to null. Otherwise, set |embedded origin| to the [=/origin=] (`"https"`, |embedded host|, `null`, `null`).
1. Return the [=origin-bound one-time code=] (|top-level origin|, |embedded origin|, |code|).

To <dfn type="abstract-op">extract a marked token</dfn> from |string| at |position| with [=code point=] |marker|, run the following steps:

1. If |position| points past the end of |string|, return failure.
1. If the [=code point=] at |position| within |string| is not |marker|, return failure.
1. Advance |position| by 1.
1. If |position| points past the end of |string|, return failure.
1. Let |token| be the result of [=collecting a sequence of code points=] which are not [=ASCII whitespace=] from |string| with |position|.
1. If |token| is the empty string, return failure.
1. Return |token|.

The <dfn type=abstract-op>last line</dfn> of |string| is the result of running these steps:

1. [=Normalize newlines=] in |string|.
1. Let |lines| be the result of <a lt="strictly split a string">strictly splitting</a> |string| on U+000A (LF).
1. Return the last item of |lines|.

<h2 id="security-considerations">Security considerations</h2>

This specification attempts to mitigate the phishing risk associated with the delivery of one-time codes over SMS by enabling User Agents to know what website the one-time code is intended for.

This specification does not attempt to mitigate other risks associated with the delivery of one-time codes over SMS, such as SMS spoofing, SIM swapping, SIM cloning, ISMI-catchers, or interception of the message by an untrusted party.

Sites would do well to consider using non-SMS technologies such as [[WEBAUTHN]] for authentication or verification.

<h2 id="privacy-considerations">Privacy considerations</h2>

Any party which has access to a user's SMS messages (such as the user's cellular carrier, mobile operating system, or anyone who intercepted the message) can learn that the user has an account on the service identified in an [=origin-bound one-time code message=] delivered over SMS.

On some platforms, User Agents might need access to all incoming SMS messages???even messages which are not [=origin-bound one-time code messages=]???in order to support the autofilling of [=origin-bound one-time codes=] delivered over SMS in [=origin-bound one-time code messages=].

<h2 id="acknowedgements" class="no-num">Acknowledgements</h2>

Many thanks to
Aaron Parecki,
Elaine Knight,
Eric Shepherd,
Eryn Wells,
Jay Mulani,
Ricky Mondello,
and
Steven Soneff
for their valuable feedback on this proposal.
