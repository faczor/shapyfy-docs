---
tags:
  - feature
  - auth
name: Authentication
status: "[[Statuses/Done]]"
---

# Authentication Feature

> **Navigation**: See [00-app-overview.md](00-app-overview.md) for complete navigation flows and integration points
> **Design Specs**:
> - [Auth Touchpoints](../designer/4.01-auth-touchpoints.md)
> - [Auth Flow](../designer/4.02-auth-flow.md)

## Overview

### Purpose
The Authentication feature enables users to create accounts and sync their workout data to the cloud. It uses a progressive optional approach with "Early Access" framing to make users feel like valued insiders helping build the app, not data points being harvested.

### User Problem
Users face several concerns when asked to create accounts:
- **Data safety**: "What if I lose my phone? What happens to my workout history?"
- **Trust**: "Why do they need my email? Is this spam?"
- **Friction**: "I just want to track workouts, why do I need an account?"
- **Value**: "What do I get in return for giving you my email?"

### Responsibilities
- Capture user emails for community building and feedback
- Provide cloud backup for workout data
- Enable multi-device sync (future feature)
- Build early access community (users who help shape the app)
- Send onboarding emails, gather feedback, conduct surveys
- Maintain offline-first architecture (accounts are optional, not required)

---

## Strategy & Philosophy

### Progressive Optional Approach
**Core Principle:** Never force account creation. Always give users the option to keep using the app locally.

**Why?**
- Competition has optional accounts (industry standard)
- Offline-first architecture (gyms have poor connectivity)
- Respects user autonomy (builds trust)
- Quality over quantity (engaged users only)

### "Early Access" Framing
**Key Message:** You're not just creating an account - you're joining an exclusive program to help build Shapyfy.

**Value Proposition:**
- ✓ Cloud backup (never lose your data)
- ✓ Multi-device sync (coming soon)
- ✓ Early access to new features (progress charts, workout programs)
- ✓ Direct line to founders (your feedback shapes the app)

### Four Touchpoint Strategy
Multiple opportunities to convert, respecting user readiness. See [Auth Touchpoints](../designer/4.01-auth-touchpoints.md) for UI details.

1. **After Demo** (15-25% conversion) - Softest ask, captures enthusiastic early adopters
2. **After 1st Workout** (30-45% conversion) - Data protection framing, sunk cost psychology
3. **After 3rd Workout** (55-70% conversion) - Strongest ask, habit formation + fear of data loss
4. **Settings Menu** (70-85% conversion) - User-initiated, highest intent

**Total Expected Conversion:** 60-75% of users who complete 3 workouts

---

## Email Onboarding Strategy

After user creates account, they receive a series of emails designed to build community, gather feedback, and set expectations for the Early Access program.

### Email Sender Identity
**From:** [Your Name], Shapyfy Founder
**Reply-to:** [Your Email]
**Tone:** Personal, founder-to-user conversation

### Email Frequency Summary

| Timeline | Email | Purpose | Expected Reply Rate |
|----------|-------|---------|---------------------|
| Day 0 | Welcome | Set expectations, open dialogue | 15-25% |
| Day 3-5 | Feedback | Gather initial impressions | 20-30% |
| Week 2-3 | Feature preview | Show progress, build anticipation | 10-15% |
| Month 1 | Survey | Major feature prioritization | 35-50% |
| Ongoing | Feature launches | When new features ship | 5-10% |

**Total emails in first month:** 4 emails
**After month 1:** Only when you launch new features or need specific feedback (avoid spam)

---

## Developer Implementation Notes

### State Management

#### User Account State
```kotlin
data class UserAccountState(
    val isLoggedIn: Boolean = false,
    val userEmail: String? = null,
    val hasSeenTouchpoint1: Boolean = false, // After demo fork
    val hasSeenTouchpoint2: Boolean = false, // After 1st workout
    val hasSeenTouchpoint3: Boolean = false, // After 3rd workout
    val workoutsSynced: Int = 0,
    val lastSyncTimestamp: Long? = null
)
```

### OAuth Implementation

#### Google Sign-In
- Use Google OAuth library for KMP
- Scopes: email, profile
- Handle token exchange server-side
- Store tokens securely (Keychain/Keystore)

#### Apple Sign-In
- Required on iOS if Google OAuth is present
- Use Apple Sign-In SDK
- Handle authorization code exchange
- Support "Hide My Email" feature

### Data Sync Strategy

#### Sync Trigger Points
1. **Account creation**: Immediate sync of all local workouts
2. **App foreground**: Check for pending syncs
3. **After workout save**: Background sync (with retry logic)
4. **Manual sync**: Settings option for user-initiated sync

#### Conflict Resolution
**Strategy:** Last write wins (for MVP)
- No complex merge logic initially
- Future enhancement: Show conflict resolution UI

### Analytics Events

Track these events for optimization:

```kotlin
// Touchpoint impressions
trackEvent("touchpoint_1_shown")
trackEvent("touchpoint_2_shown")
trackEvent("touchpoint_3_shown")

// Conversion events
trackEvent("account_created", mapOf(
    "method" to "google|apple|email",
    "source_touchpoint" to "1|2|3|settings"
))
```

---

## Success Metrics

### Conversion Goals
- **Touchpoint 1**: 15-25% conversion rate
- **Touchpoint 2**: 30-45% conversion rate
- **Touchpoint 3**: 55-70% conversion rate
- **Overall** (users who complete 3 workouts): 60-75% account creation

### Quality Metrics
- **Fake emails**: < 5% of signups
- **Email bounces**: < 2% of signups
- **Unsubscribe rate**: < 3% per email

---

## Future Enhancements

### Phase 2: Account Management
- Edit profile (name, profile picture)
- Change password
- Delete account
- Email preferences
- Privacy settings

### Phase 3: Social Features
- Share workout summaries
- Follow friends
- Workout leaderboards (requires accounts)
- "Train with friends" challenges

### Phase 4: Premium Tier
- Freemium conversion funnel
- Subscription management
- Premium-only features gated behind account + payment
