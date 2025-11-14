# Analytics & Monitoring Setup Guide for JamesFrancisThorpe.com

This comprehensive guide will help you set up analytics, SEO monitoring, and performance tracking to measure the success of your SEO/AEO improvements and make data-driven decisions.

---

## Table of Contents

1. [Google Analytics 4 Setup](#google-analytics-4-setup)
2. [Google Search Console](#google-search-console)
3. [Schema Markup Validation](#schema-markup-validation)
4. [SEO Monitoring Tools](#seo-monitoring-tools)
5. [AEO Tracking](#aeo-tracking)
6. [Social Media Analytics](#social-media-analytics)
7. [Key Metrics to Track](#key-metrics-to-track)
8. [Reporting & Dashboards](#reporting--dashboards)

---

## Google Analytics 4 Setup

### Why GA4?
- Free, comprehensive analytics
- Real-time visitor data
- User journey tracking
- Event-based tracking
- Integration with Google Search Console

### Step-by-Step Setup

#### 1. Create GA4 Property

1. Go to [analytics.google.com](https://analytics.google.com)
2. Click "Admin" → "Create Property"
3. Enter property name: "Jim Thorpe Website"
4. Set timezone and currency
5. Click "Create"

#### 2. Set Up Data Stream

1. Choose "Web" platform
2. Enter website URL: `https://www.jamesfrancisthorpe.com`
3. Stream name: "Jim Thorpe Main Site"
4. Enable "Enhanced measurement" (recommended)
5. Click "Create stream"
6. Copy the "Measurement ID" (format: G-XXXXXXXXXX)

#### 3. Install Tracking Code

Add this to the `<head>` section of ALL pages:

```html
<!-- Google Analytics 4 -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX', {
    'anonymize_ip': true,  // Privacy-friendly
    'allow_google_signals': false,
    'allow_ad_personalization_signals': false
  });
</script>
```

#### 4. Set Up Enhanced Tracking

Add custom events for better insights:

```html
<script>
// Track newsletter signups
document.querySelector('.newsletter-form').addEventListener('submit', function() {
  gtag('event', 'newsletter_signup', {
    'event_category': 'engagement',
    'event_label': 'Newsletter Form'
  });
});

// Track social shares
document.querySelectorAll('.social-btn').forEach(btn => {
  btn.addEventListener('click', function(e) {
    const platform = this.classList.contains('facebook') ? 'Facebook' :
                     this.classList.contains('twitter') ? 'Twitter' :
                     this.classList.contains('linkedin') ? 'LinkedIn' : 'Email';

    gtag('event', 'share', {
      'method': platform,
      'content_type': 'article',
      'content_id': window.location.pathname
    });
  });
});

// Track article reading completion
window.addEventListener('scroll', function() {
  const scrollPercentage = (window.scrollY / (document.body.scrollHeight - window.innerHeight)) * 100;

  if (scrollPercentage > 90 && !window.articleCompleted) {
    window.articleCompleted = true;
    gtag('event', 'article_complete', {
      'event_category': 'engagement',
      'event_label': document.title,
      'value': Math.round(scrollPercentage)
    });
  }
});

// Track outbound links
document.querySelectorAll('a[href^="http"]').forEach(link => {
  if (!link.href.includes('jamesfrancisthorpe.com')) {
    link.addEventListener('click', function() {
      gtag('event', 'click', {
        'event_category': 'outbound',
        'event_label': this.href
      });
    });
  }
});
</script>
```

#### 5. Verify Installation

1. Go to GA4 → "Reports" → "Realtime"
2. Visit your website
3. You should see yourself in real-time visitors
4. Test events by clicking social share buttons

---

## Google Search Console

### Why Search Console?
- Monitor search performance
- Track keyword rankings
- Identify crawl errors
- Submit sitemaps
- Monitor Core Web Vitals
- Track rich result performance

### Step-by-Step Setup

#### 1. Add Property

1. Go to [search.google.com/search-console](https://search.google.com/search-console)
2. Click "Add Property"
3. Choose "URL prefix" method
4. Enter: `https://www.jamesfrancisthorpe.com`

#### 2. Verify Ownership

**Method 1 - HTML File Upload:**
1. Download verification file
2. Upload to website root
3. Click "Verify"

**Method 2 - HTML Tag:**
```html
<meta name="google-site-verification" content="your-verification-code-here">
```

**Method 3 - DNS Record:**
Add TXT record to your DNS settings

#### 3. Submit Sitemap

1. Go to "Sitemaps" in left menu
2. Enter: `https://www.jamesfrancisthorpe.com/sitemap.xml`
3. Click "Submit"
4. Wait 24-48 hours for indexing

#### 4. Link to GA4

1. In Search Console, go to "Settings"
2. Click "Google Analytics property"
3. Select your GA4 property
4. This enables integrated reporting

---

## Schema Markup Validation

### Essential Validation Tools

#### 1. Google Rich Results Test

1. Go to [search.google.com/test/rich-results](https://search.google.com/test/rich-results)
2. Enter each page URL:
   - Homepage: `https://www.jamesfrancisthorpe.com/`
   - Story page: `https://www.jamesfrancisthorpe.com/story`
   - Articles page: `https://www.jamesfrancisthorpe.com/articles`
   - Individual articles
3. Review results for errors/warnings
4. Fix any issues found

**Expected Rich Results:**
- ✅ Person schema (homepage)
- ✅ FAQPage schema (homepage, articles)
- ✅ Article schema (story, articles)
- ✅ BreadcrumbList schema (all pages)

#### 2. Schema.org Validator

1. Go to [validator.schema.org](https://validator.schema.org/)
2. Test each page
3. Verify all schema types are valid
4. Check for required properties

#### 3. Monitor in Search Console

1. Go to Search Console → "Experience" → "Search Appearance"
2. Review:
   - FAQ rich results
   - Article rich results
   - Breadcrumbs
3. Fix any errors reported

---

## SEO Monitoring Tools

### 1. Google Search Console (Free)

**Track:**
- Search queries driving traffic
- Click-through rates (CTR)
- Average position for keywords
- Impressions and clicks

**Key Reports:**
- Performance → Queries
- Performance → Pages
- Coverage (indexing issues)
- Core Web Vitals

### 2. Bing Webmaster Tools (Free)

Setup steps:
1. Go to [bing.com/webmasters](https://www.bing.com/webmasters)
2. Add site: `https://www.jamesfrancisthorpe.com`
3. Verify ownership
4. Submit sitemap

**Benefits:**
- 33% of US search market share
- SEO insights from Bing
- Additional keyword data

### 3. SEO Browser Extensions (Free)

#### SEOquake
- Shows SEO metrics on search results
- Page analysis
- Keyword density
- Internal/external links

#### MozBar
- Domain authority
- Page authority
- Link metrics

#### Lighthouse (Chrome DevTools)
```
F12 → Lighthouse Tab → Generate Report
```
Tracks:
- Performance score
- SEO score
- Accessibility score
- Best practices

---

## AEO Tracking

### Monitor AI Search Presence

#### 1. ChatGPT Citation Tracking

**Manual Monitoring:**
1. Ask ChatGPT: "Who was Jim Thorpe?"
2. Check if your website is cited
3. Note what information is used
4. Track monthly

**What to Ask:**
- "Who was Jim Thorpe?"
- "Why were Jim Thorpe's Olympic medals taken away?"
- "When did Jim Thorpe get his medals back?"
- "What is Jim Thorpe's Native American heritage?"

#### 2. Perplexity.ai Tracking

1. Visit [perplexity.ai](https://www.perplexity.ai)
2. Ask same questions as ChatGPT
3. Check for citations
4. Monitor monthly

#### 3. Google AI Overviews

Monitor if your content appears in:
- Featured Snippets
- People Also Ask boxes
- AI-generated summaries

**Track in Search Console:**
- Performance → Search Appearance
- Filter by "Rich Results"
- Monitor FAQ and Article rich results

#### 4. Voice Search Testing

**Test on devices:**
- Google Assistant: "Hey Google, who was Jim Thorpe?"
- Siri: "Hey Siri, tell me about Jim Thorpe"
- Alexa: "Alexa, who was Jim Thorpe?"

**Track:**
- If your content is used
- How it's presented
- Accuracy of information

---

## Social Media Analytics

### 1. Facebook Sharing Debugger

1. Go to [developers.facebook.com/tools/debug](https://developers.facebook.com/tools/debug/)
2. Enter each page URL
3. Verify Open Graph tags display correctly
4. Click "Scrape Again" if needed

**Check:**
- Title displays correctly
- Description is compelling
- Image loads properly
- No errors shown

### 2. Twitter Card Validator

1. Go to [cards-dev.twitter.com/validator](https://cards-dev.twitter.com/validator)
2. Enter each page URL
3. Verify Twitter Card preview
4. Check image, title, description

### 3. LinkedIn Post Inspector

1. Go to [linkedin.com/post-inspector](https://www.linkedin.com/post-inspector/)
2. Enter URL
3. Verify preview looks good
4. Clear cache if needed

### 4. Social Share Tracking

In GA4, track social shares:

```javascript
// Already added in GA4 setup section above
gtag('event', 'share', {
  'method': 'Facebook',  // or Twitter, LinkedIn
  'content_type': 'article',
  'content_id': '/story'
});
```

Monitor in GA4:
- Events → share
- See which content is shared most
- Which platforms drive shares

---

## Key Metrics to Track

### SEO Metrics (Monthly)

| Metric | Tool | Target | Priority |
|--------|------|--------|----------|
| **Organic Traffic** | GA4 | ↑ 20% MoM | High |
| **Organic CTR** | Search Console | > 5% | High |
| **Average Position** | Search Console | Top 10 | High |
| **Indexed Pages** | Search Console | All pages | High |
| **Core Web Vitals** | Search Console | All "Good" | High |
| **Backlinks** | Search Console | ↑ Monthly | Medium |

### AEO Metrics (Monthly)

| Metric | Method | Target | Priority |
|--------|--------|--------|----------|
| **Featured Snippets** | Manual Search | 3+ queries | High |
| **FAQ Rich Results** | Search Console | 100% | High |
| **ChatGPT Citations** | Manual Test | 1+ citations | Medium |
| **Perplexity Citations** | Manual Test | 1+ citations | Medium |
| **Voice Search Ranking** | Manual Test | Mentioned | Low |

### Engagement Metrics (Weekly)

| Metric | Tool | Target | Priority |
|--------|------|--------|----------|
| **Bounce Rate** | GA4 | < 50% | High |
| **Avg. Time on Page** | GA4 | > 2 min | High |
| **Pages per Session** | GA4 | > 2.5 | Medium |
| **Newsletter Signups** | GA4 Events | ↑ Weekly | High |
| **Social Shares** | GA4 Events | 10+ / week | Medium |
| **Article Completion** | GA4 Events | > 60% | Medium |

### Performance Metrics (Weekly)

| Metric | Tool | Target | Priority |
|--------|------|--------|----------|
| **PageSpeed Score** | PageSpeed Insights | > 90 | High |
| **LCP** | Search Console | < 2.5s | High |
| **CLS** | Search Console | < 0.1 | High |
| **Mobile Usability** | Search Console | 0 errors | High |

---

## Reporting & Dashboards

### 1. Google Analytics 4 Dashboard

Create custom dashboard:

1. Go to GA4 → "Explore" → "Blank"
2. Add these widgets:
   - Organic traffic trend (line chart)
   - Top pages (table)
   - User demographics (bar chart)
   - Real-time users (scorecard)
   - Events (table: newsletter, shares, completion)

3. Save as "Jim Thorpe - Monthly Overview"

### 2. Google Data Studio (Free)

Create visual reports:

1. Go to [datastudio.google.com](https://datastudio.google.com)
2. Create new report
3. Connect data sources:
   - GA4
   - Search Console
4. Add charts:
   - Organic traffic over time
   - Top keywords
   - Page performance
   - Core Web Vitals

**Sample Dashboard Layout:**
```
┌─────────────────────────────────────┐
│  Jim Thorpe Website - SEO Dashboard│
├─────────────────────────────────────┤
│ Organic Traffic (30 days)           │
│ [Line Chart]                        │
├──────────────┬──────────────────────┤
│ Top Keywords │ Top Pages            │
│ [Table]      │ [Table]              │
├──────────────┴──────────────────────┤
│ Core Web Vitals                     │
│ [Scorecard: LCP, CLS, FID]          │
└─────────────────────────────────────┘
```

### 3. Weekly SEO Report Template

```markdown
# Weekly SEO Report - Week of [Date]

## Traffic
- Organic visits: [number] (↑/↓ X% vs last week)
- New users: [number]
- Returning users: [number]

## Rankings
- Top performing keywords:
  1. [keyword] - Position #[X]
  2. [keyword] - Position #[X]
  3. [keyword] - Position #[X]

## Engagement
- Avg. time on site: [time]
- Bounce rate: [percentage]
- Newsletter signups: [number]
- Social shares: [number]

## Technical
- Crawl errors: [number]
- Core Web Vitals: [status]
- New indexed pages: [number]

## Goals Met This Week
- [ ] Goal 1
- [ ] Goal 2

## Next Week Focus
- Action item 1
- Action item 2
```

### 4. Monthly SEO Report Template

```markdown
# Monthly SEO Report - [Month Year]

## Executive Summary
[2-3 sentence overview of performance]

## Traffic Analysis
- Total organic visitors: [number] (±X% MoM)
- New visitors: [number]
- Sessions: [number]
- Pageviews: [number]

## Top Performing Content
1. [Page name] - [visits]
2. [Page name] - [visits]
3. [Page name] - [visits]

## SEO Performance
- Average search position: #[X]
- Click-through rate: [X]%
- Total impressions: [number]
- Total clicks: [number]

## AEO Performance
- Featured snippets: [number]
- FAQ rich results: [number]
- AI citations detected: [number]

## Technical Health
- Site speed score: [number]/100
- Mobile usability: [status]
- Core Web Vitals: All Green/Some Issues
- Indexation: [X]/[Y] pages

## Goals Achieved
- ✅ [Goal 1]
- ✅ [Goal 2]
- ⏳ [Goal 3 - in progress]

## Recommendations for Next Month
1. [Recommendation]
2. [Recommendation]
3. [Recommendation]
```

---

## Monthly Checklist

### Week 1:
- [ ] Review GA4 traffic report
- [ ] Check Search Console performance
- [ ] Validate schema markup on all pages
- [ ] Test website speed (PageSpeed Insights)
- [ ] Review Core Web Vitals

### Week 2:
- [ ] Test AI citations (ChatGPT, Perplexity)
- [ ] Check featured snippets presence
- [ ] Review social media analytics
- [ ] Monitor competitor rankings
- [ ] Update content if needed

### Week 3:
- [ ] Analyze top-performing content
- [ ] Review keyword rankings
- [ ] Check for crawl errors
- [ ] Test on mobile devices
- [ ] Review user engagement metrics

### Week 4:
- [ ] Create monthly SEO report
- [ ] Set goals for next month
- [ ] Plan content updates
- [ ] Review and respond to issues
- [ ] Archive reports for trends

---

## Success Indicators

### 3-Month Goals

**Traffic:**
- 50% increase in organic traffic
- 1,000+ monthly visitors
- 60%+ new visitor rate

**SEO:**
- 3+ featured snippets
- Average position in top 10 for main keywords
- 100% pages indexed
- 95+ PageSpeed score

**AEO:**
- 1+ ChatGPT citation
- 1+ Perplexity citation
- All FAQ schema validated
- 5+ rich results in Search Console

**Engagement:**
- 50+ newsletter signups/month
- 100+ social shares/month
- <40% bounce rate
- 3+ minutes average time on site

---

## Tools Summary

### Free Tools:
- ✅ Google Analytics 4
- ✅ Google Search Console
- ✅ Bing Webmaster Tools
- ✅ Google PageSpeed Insights
- ✅ Schema.org Validator
- ✅ Facebook Sharing Debugger
- ✅ Twitter Card Validator
- ✅ Chrome DevTools / Lighthouse

### Paid Tools (Optional):
- SEMrush (keyword research, competitor analysis)
- Ahrefs (backlink analysis, keyword tracking)
- Moz Pro (comprehensive SEO suite)
- Screaming Frog (technical SEO audits)

---

## Resources

### Documentation:
- [GA4 Help Center](https://support.google.com/analytics)
- [Search Console Help](https://support.google.com/webmasters)
- [Schema.org Documentation](https://schema.org/docs/documents.html)

### Learning:
- [Google Analytics Academy](https://analytics.google.com/analytics/academy/)
- [Google Search Central](https://developers.google.com/search)
- [Moz SEO Learning Center](https://moz.com/learn/seo)

---

**Last Updated:** January 2025
**Next Review:** April 2025

This comprehensive analytics setup will give you complete visibility into your website's performance, SEO success, and user engagement, allowing you to make data-driven decisions and continuously improve.
