---
layout: post
title: "RichTexture"
date: 2018-05-04 9:41
tags: [iPhone, Swift, Open Source, RichTexture]
categories: [apple-dev]
---

Today I'm [releasing](https://itunes.apple.com/app/richtexture/id1376116077) and [open sourcing RichTexture](https://github.com/stevemoser/richtexture), a rich text editor for iOS. I started this project after discovering Louis D'hauwe's open source plain text editor for iOS, [Textor](https://github.com/louisdh/textor). I forked his editor and replaced plain text editing with rich text editing. RichTexture is the first open source iOS rich text editor that supports saving and opening files through the system documents browser. This is important because even though Apple's Pages app supports opening rich text (.rtf and .rtfd) files, it only allows for saving files in the Pages (.pages) file format.

RichTexture is a few system APIs glued together:

    RichTexture = Textor (UIDocument + UIDocumentBrowserViewController + UITextView) + NSAttributedString (storing rich text for UITextView) + NSFileWrapper (converting NSAttributedString to an .rtf file)

RichTexture is available on the [App Store](https://itunes.apple.com/app/richtexture/id1376116077).
