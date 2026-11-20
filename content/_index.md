---
# Leave the homepage title empty to use the site title
title: ''
summary: ''
date: 2022-10-24
type: landing

design:
  # Default section spacing
  spacing: '2rem'

sections:
  - block: profile-highlight
    id: home-overview
    content:
      # Choose a user profile to display (from `data/authors/`)
      username: me
      text: ''
      # Show a call-to-action button under your biography? (optional)
      button:
        text: View Posts
        url: /blog/
      headings:
        about: About Me
        education: Experience
        interests: Interests
      # Right column top: Pinned highlight works (add `featured: true` in post front matter)
      highlights:
        title: Highlight Works
        empty_text: 'Nothing for now : )'
        empty_hint: '在文章或项目的 front matter 中添加 featured: true 即可置顶展示到这里。'
        filters:
          folders:
            - blog
          featured_only: true
        count: 4
      # Right column below highlights: Recent posts
      recent:
        enable: true
        title: Recent Posts
        filters:
          folders:
            - blog
          exclude_featured: true
        count: 10
        archive:
          enable: true
          link: /blog/
    design:
      spacing:
        padding: ['2rem', '0', '3rem', '0']
      gradient_mesh:
        enable: true
      avatar:
        shape: circle # Options: circle (default), square, rounded
      highlights:
        columns: 2
        show_image: true # 有配封面图时展示，未配则自动隐藏
      recent:
        columns: 2
        show_image: true # 按文章自身配置决定：有配封面图（含动图）才显示，未配则为纯文字卡片
---
