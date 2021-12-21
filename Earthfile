FROM --platform=linux/amd64 busybox

ESBUILD:
    COMMAND
    ARG input
    ARG output
    COPY $input $output
    SAVE ARTIFACT ./*

esbuild-foo:
    DO +ESBUILD --input=./source.ts --output=./output.js

deps:
    COPY foo.bar ./
    COPY bar.baz ./
    SAVE ARTIFACT ./*

baz:
    COPY baz ./
    SAVE ARTIFACT ./*

someotherrandomtarget:
    FROM +baz
    COPY baz .
    SAVE IMAGE --push jgilli/earthly-multiplatform-repro-someotherrandomtarget:latest

foo-context:
    FROM +deps
    COPY +deps/foo.bar .
    SAVE ARTIFACT ./foo.bar

foo:
    ARG foo
    COPY +esbuild-foo/output.js .
    SAVE IMAGE --push jgilli/earthly-multiplatform-repro-foo:latest

bar-context:
    FROM +deps
    COPY +deps/bar.baz .
    SAVE ARTIFACT ./bar.baz

bar:
    ARG bar
    SAVE IMAGE --push jgilli/earthly-multiplatform-repro-bar:latest

images:
    BUILD +foo
    BUILD +bar