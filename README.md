# contest.go

## Introduction

contest.go is a simple and naive Go library to help you
with writing and testing solutions for competitive programming problems.

For problems with a fixed answer for each test case,
you can use the standard `go test` command to test your solutions.
You can also use `contest-cli` to prepare your solution template
along with test cases retrieved from the web site.

Supported competitive programming services:
- [AtCoder](https://atcoder.jp)

## Installation

Just run the following to install `contest-cli`:

```console
$ go get github.com/yaegashi/contest.go/cmd/contest-cli
```

## Preparing a solution template with test cases

First, you need to make a Go module for your solutions:

```console
$ git init solutions
Initialized empty Git repository in /home/yaegashi/solutions/.git/
$ cd solutions/
$ go mod init solutions
go: creating new go.mod: module solutions
```

You can prepare a generic solution dir
by running `contest-cli new` with a dir name:

```console
$ contest-cli new foo
2020/06/28 16:20:46 I: Created foo/main.go
2020/06/28 16:20:46 I: Created foo/main_test.go
2020/06/28 16:20:46 I: Created foo/sample1.in.txt
2020/06/28 16:20:46 I: Created foo/sample1.out.txt
```

You are ready to run `go test` with the `sample1` test case:
```console
$ go test -v ./foo
go: finding module for package github.com/yaegashi/contest.go/tester
go: downloading github.com/yaegashi/contest.go v0.0.1
go: found github.com/yaegashi/contest.go/tester in github.com/yaegashi/contest.go v0.0.1
=== RUN   TestContest
=== RUN   TestContest/0:sample1.in.txt
--- PASS: TestContest (0.00s)
    --- PASS: TestContest/0:sample1.in.txt (0.00s)
PASS
ok      solutions/foo   1.319s
```

You can prepare solution dirs for AtCoder contests
by `contest-cli atcoder new`:
```console
$ contest-cli atcoder new abc069
2020/07/05 00:41:24 I: Fetching AtCoder contest abc069
2020/07/05 00:41:24 I: Created abc069/a/main.go
2020/07/05 00:41:24 I: Created abc069/a/main_test.go
2020/07/05 00:41:24 I: Created abc069/a/sample1.in.txt
2020/07/05 00:41:24 I: Created abc069/a/sample1.out.txt
2020/07/05 00:41:24 I: Created abc069/a/sample2.in.txt
2020/07/05 00:41:24 I: Created abc069/a/sample2.out.txt
2020/07/05 00:41:24 I: Created abc069/b/main.go
2020/07/05 00:41:24 I: Created abc069/b/main_test.go
2020/07/05 00:41:24 I: Created abc069/b/sample1.in.txt
2020/07/05 00:41:24 I: Created abc069/b/sample1.out.txt
2020/07/05 00:41:24 I: Created abc069/b/sample2.in.txt
2020/07/05 00:41:24 I: Created abc069/b/sample2.out.txt
2020/07/05 00:41:24 I: Created abc069/b/sample3.in.txt
2020/07/05 00:41:24 I: Created abc069/b/sample3.out.txt
2020/07/05 00:41:24 I: Created abc069/c/main.go
2020/07/05 00:41:24 I: Created abc069/c/main_test.go
2020/07/05 00:41:24 I: Created abc069/c/sample1.in.txt
2020/07/05 00:41:24 I: Created abc069/c/sample1.out.txt
2020/07/05 00:41:24 I: Created abc069/c/sample2.in.txt
2020/07/05 00:41:24 I: Created abc069/c/sample2.out.txt
2020/07/05 00:41:24 I: Created abc069/c/sample3.in.txt
2020/07/05 00:41:24 I: Created abc069/c/sample3.out.txt
2020/07/05 00:41:24 I: Created abc069/c/sample4.in.txt
2020/07/05 00:41:24 I: Created abc069/c/sample4.out.txt
2020/07/05 00:41:24 I: Created abc069/c/sample5.in.txt
2020/07/05 00:41:24 I: Created abc069/c/sample5.out.txt
2020/07/05 00:41:24 I: Created abc069/d/main.go
2020/07/05 00:41:24 I: Created abc069/d/main_test.go
2020/07/05 00:41:24 I: Created abc069/d/sample1.in.txt
2020/07/05 00:41:24 I: Created abc069/d/sample1.out.txt
2020/07/05 00:41:24 I: Created abc069/d/sample2.in.txt
2020/07/05 00:41:24 I: Created abc069/d/sample2.out.txt
2020/07/05 00:41:24 I: Created abc069/d/sample3.in.txt
2020/07/05 00:41:24 I: Created abc069/d/sample3.out.txt
```

The example above shows it creates 4 solution dirs
for [AtCoder Beginner Contest 069](https://atcoder.jp/contests/abc069)
with solution templates and test cases.
Example solutions with them can be found at
[diligence.go repository](https://github.com/yaegashi/diligence.go/tree/master/atcoder/abc069).

You can also prepare a single solution dir and specify an output dir for that:

```console
$ contest-cli atcoder new abc069 d -d abc069/d2
2020/07/05 00:57:12 I: Fetching AtCoder contest abc069
2020/07/05 00:57:13 I: Created abc069/d2/main.go
2020/07/05 00:57:13 I: Created abc069/d2/main_test.go
2020/07/05 00:57:13 I: Created abc069/d2/sample1.in.txt
2020/07/05 00:57:13 I: Created abc069/d2/sample1.out.txt
2020/07/05 00:57:13 I: Created abc069/d2/sample2.in.txt
2020/07/05 00:57:13 I: Created abc069/d2/sample2.out.txt
2020/07/05 00:57:13 I: Created abc069/d2/sample3.in.txt
2020/07/05 00:57:13 I: Created abc069/d2/sample3.out.txt
```

## Limitations

Currently contest.go's tester judge implements very simple line-by-line matcher only.
You need to make solutions that generates outputs strictly matching in all lines.

This means you cannot test problems that should accept multiple possible answers,
for example, [ABC069 Problem D](https://atcoder.jp/contests/abc069/tasks/arc080_b):

```console
$ go test -v .
=== RUN   TestContest
=== RUN   TestContest/0:sample1.in.txt
    TestContest/0:sample1.in.txt: tester.go:93: Wrong answer:
        --- want
        +++ got
        @@ -1,2 +1,2 @@
         1 1
        -2 3
        +3 2
=== RUN   TestContest/1:sample2.in.txt
    TestContest/1:sample2.in.txt: tester.go:93: Wrong answer:
        --- want
        +++ got
        @@ -1,3 +0,0 @@
        -1 4 4 4 3
        -2 5 4 5 3
        -2 5 5 5 3
        @@ -0,0 +1,3 @@
        +1 2 2 3 3
        +4 4 4 4 3
        +5 5 5 5 5
=== RUN   TestContest/2:sample3.in.txt
--- FAIL: TestContest (0.00s)
    --- FAIL: TestContest/0:sample1.in.txt (0.00s)
    --- FAIL: TestContest/1:sample2.in.txt (0.00s)
    --- PASS: TestContest/2:sample3.in.txt (0.00s)
FAIL
FAIL    github.com/yaegashi/diligence.go/atcoder/abc069/d       0.207s
FAIL
```

## TODO

- Support more competitive programming services
- Custom judge integration for problems with multiple possible answers
- Ability to submit solutions to competitive programming services
