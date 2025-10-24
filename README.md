# Forte Hacks Leaderboard

This is a leaderboard tracking and rewarding the most active projects during [Forte Hacks](https://link.flow.com/ForteHacksDevRel) from October 1 - 31st. Builders that have the most unique commit days across the month will recieve a drop of bonus rewards!

It's time to go WTF (with the Flow) again with Forte Hacks!

## 🤝 Bonus Rewards!

Coming soon


## Leaderboard

Leaderboard coming soon

## 🚀 How to Register Your Team

To participate in the Forte Hacks Leaderbaord and start earning points, you need to register your team in the `registry.yaml` file.

### 📋 Registration Requirements

Your team's projects must meet **at least one** of the following criteria:

1. **"Built on Flow"** - Your projects clearly state in their README.md that they are built on Flow, and if there are any contracts, list the contract addresses deployed on Flow.

2. **With Flow Components** - Your projects use:
   - Cadence Language
   - `@onflow/fcl` or `@onflow/react-sdk` (in package.json)
   - Go SDK (URL in go.mod)
   - Flow public EVM endpoints (in Solidity configuration files)

### 📝 Registration Template

Copy the following structure, modify it to your needs, and add it to the end of `registry.yaml`:

```yaml
-
  name: Your Team Name (Displayed in Leaderboard)
  github:
    # Your team members' GitHub usernames for tracking PR status to onflow
    - username1
    - username2
  repos:
    # Your team's repositories - all commits to these repos will be tracked
    - https://github.com/your/repo_1
    - https://github.com/your/repo_2
  wallets:
    # Your team's wallets to accept Flow Rewards
    evm: "0x0000000000000000000000000000000000000000"
    flow: "0x0000000000000001"
```

Note, for rewards store points use an EVM address like [Metamask](https://developers.flow.com/build/evm/using) or [Flow Wallet](https://wallet.flow.com/)'s EVM accounts.

### ✅ Validation Requirements

The registration system will automatically validate:

- **Team Name**: Required, Must be a non-empty string
- **GitHub Handles**: Required, Must be valid, accessible GitHub usernames
- **Repository URLs**: Required, Must be valid, accessible GitHub repositories
- **Wallet Addresses**:
  - EVM: Required, Must start with `0x` and be 42 characters long (0x + 40 hex chars)
  - Flow: Required, Must start with `0x` and be 18 characters long (0x + 16 hex chars)

### 🔍 Continuous Monitoring

After successful registration, we will continuously monitor and scan your GitHub activity data to calculate user points.


## 🏆 Scoring System

### How Points Are Calculated

- Points are generated through many different sources. The higher your points, the more likely you are to win prizes and funds. To maximize your score make sure you create to push meaningful commits regularly. 


## 📚 Additional Resources

- [Flow Documentation](https://developers.flow.com/)
- [Cadence Language Reference](https://cadence-lang.org/)

## FAQ
- Can I use existing projects or do they need to be brand new? Both new and existing projects are eligible!
- How often does the leaderboard update? Currently the leaderboard updates once every 24 hours on a daily script, if you're not seeing your points check back tomorrow.
---

**Note**: All contributions to onflow repositories and registered Flow-related projects are automatically tracked. Make sure your GitHub username is correctly listed in your team's registration to receive proper credit for your contributions.
