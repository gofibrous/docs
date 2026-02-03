# CLAUDE.md - Fibrous Finance Documentation

## Project Overview

Fibrous Finance documentation site built with **Mintlify** (docs.json v2 config). Live at `https://docs.fibrous.finance`.

- **Framework**: Mintlify (MDX-based)
- **Config**: `docs.json` (not legacy `mint.json`)
- **API Base**: `https://api.fibrous.finance`
- **Repo**: `github.com/gofibrous/docs`

## Repository Structure

```
docs/
├── docs.json                          # Mintlify config (nav, theme, API)
├── essentials/                        # Overview pages (FAQ, intro)
├── api-reference/                     # API docs (endpoints, chains, starknet)
│   ├── endpoints/                     # Core V1 + V2 endpoints
│   ├── chains-tokens-contracts/       # Supported chains, tokens, contracts
│   ├── starknet/                      # Starknet-specific API
│   ├── base/                          # ORPHANED - not in nav
│   ├── hyperevm/                      # ORPHANED - not in nav
│   ├── scroll/                        # DEPRECATED - legacy Scroll files
│   └── citrea/                        # Partially orphaned
├── fibrous-solutions/                 # UI guides (interface, widget)
│   └── fibrous-interface/             # Wallet, swapping, batch, arena
├── integrate-best-trading/            # SDK + smart contracts
│   ├── fibrous-sdk/                   # TypeScript SDK (in nav)
│   ├── fibrous-smart-contracts/       # Contract docs (partial nav)
│   └── fibrous-api/                   # ORPHANED API docs
├── fibrous-limit-order/               # ORPHANED - 4 files not in nav
├── developer-resources/               # ORPHANED - smart-contracts.mdx
├── external-links/                    # Links to app, social, GitHub
├── images/ logo/ icons/               # Assets
└── quickstart.mdx                     # ORPHANED Mintlify template (not customized)
```

## Navigation Tabs (docs.json)

1. **Overview** - Get Started, Fibrous Solutions (Interface), External Links
2. **API Reference** - Getting Started, Chains/Tokens, Core Endpoints (V1+V2), Starknet API, Smart Contracts
3. **SDK Reference** - Getting Started, Core Functions, Reference

## Supported Networks

| Network   | Status | API  | SDK  | Starknet-specific |
|-----------|--------|------|------|-------------------|
| Starknet  | Active | V1   | Yes  | Yes (dedicated)   |
| Base      | Active | V1+V2| Yes  | No                |
| Citrea    | Active | V1+V2| Yes  | No                |
| HyperEVM  | Active | V1+V2| Yes  | No                |
| Monad     | Active | V2   | Yes  | No                |
| Scroll    | REMOVED| -    | -    | -                 |

## Development

```bash
npm i -g mintlify
mintlify dev        # Local preview at localhost:3000
```

Deployments auto-trigger on push to main via Mintlify GitHub app.

## Conventions

- All content pages are `.mdx` files with YAML frontmatter (`title`, `description`)
- Navigation is defined in `docs.json` under `navigation.tabs[].groups[].pages`
- Pages not in `docs.json` navigation are hidden (orphaned) from site nav
- External links use dedicated stub files in `external-links/`
- Starknet has chain-specific endpoint docs; EVM chains share generic endpoints

## MCP Setup

```bash
# Add Mintlify docs MCP for development assistance
claude mcp add --transport http Mintlify https://mintlify.com/docs/mcp

# Add Fibrous docs MCP to test your own server
claude mcp add --transport http Fibrous https://docs.fibrous.finance/mcp
```

---

## Documentation Improvement Analysis

### Critical Issues

#### 1. 50% of Files Are Orphaned (55 of 110 files)
Files exist in the repo but are NOT in `docs.json` navigation, making them invisible to users:
- `quickstart.mdx` - Mintlify starter template, never customized for Fibrous
- `api-reference/base/` (7 files) - Chain-specific Base endpoint docs
- `api-reference/hyperevm/` (8 files) - Chain-specific HyperEVM endpoint docs
- `api-reference/citrea/` (partially) - Citrea overview references wrong paths
- `integrate-best-trading/fibrous-api/` (5 files) - Duplicate API docs
- `integrate-best-trading/introduction.mdx` - Orphaned intro page
- `fibrous-limit-order/` (4 files) - Complete limit order docs, not in nav
- `developer-resources/smart-contracts.mdx` - Duplicate contract info

**Recommendation**: Audit each orphaned file. Either add to nav or delete. The chain-specific endpoint folders (`base/`, `hyperevm/`) duplicate what the generic `endpoints/` folder already covers - likely safe to remove.

#### 2. Legacy Scroll Files Still Present
`api-reference/scroll/` (7 files) and `integrate-best-trading/fibrous-api/scroll/` (3 files) reference a network that Fibrous no longer supports. Recent commits replaced Scroll with Citrea but the old files were not cleaned up.

**Recommendation**: Delete all Scroll-related files and folders.

#### 3. quickstart.mdx Is Mintlify Boilerplate
The root `quickstart.mdx` is the unmodified Mintlify starter kit template. It references pages like `/essentials/markdown`, `/essentials/code`, `/essentials/images` which don't exist. It's not in the navigation.

**Recommendation**: Either delete it or replace with an actual Fibrous quickstart guide.

#### 4. ~~README.md Is Mintlify Starter Template~~ (FIXED)
README.md has been updated with Fibrous-specific content.

### Medium Issues

#### 5. Incomplete Content
- `fibrous-solutions/fibrous-widget.mdx` ends with "More information coming..." - installation section only, no usage examples
- `api-reference/chains-tokens-contracts/supported-chains.mdx` has TBD values for HyperEVM block time and Citrea RPC endpoint
- `api-reference/endpoints/route.mdx` has TBD for Citrea average block time

**Recommendation**: Complete the widget docs or remove from nav. Fill in TBD values.

#### 6. Fragmented Documentation
Same topics documented in multiple places:
- **API docs**: `api-reference/endpoints/`, chain-specific folders, AND `integrate-best-trading/fibrous-api/`
- **Smart contracts**: `api-reference/chains-tokens-contracts/contract-addresses.mdx`, `integrate-best-trading/fibrous-smart-contracts/`, AND `developer-resources/smart-contracts.mdx`

**Recommendation**: Consolidate to single authoritative locations. Keep the nav-referenced versions, delete duplicates.

#### 7. Limit Order Feature Hidden
`fibrous-limit-order/` has 4 well-written pages (introduction, creating orders, managing orders, FAQ) but none are in the navigation. The FAQ references limit orders as a feature.

**Recommendation**: Add to Overview tab under "Advanced Features" or create a dedicated group.

#### 8. Missing Metadata
14 files lack `description` frontmatter. This affects SEO and MCP search quality.

**Recommendation**: Add descriptions to all pages, especially API reference pages.

### Structural Improvements

#### 9. No Clear Onboarding Path
The Overview tab starts with "What is Fibrous?" then FAQ. There's no quickstart or getting-started guide that walks a developer through their first integration.

**Recommendation**: Create a proper quickstart page in the Overview tab: "Get your first swap working in 5 minutes" covering API key (if needed), first API call, and first SDK usage.

#### 10. ~~V2 Migration Could Be More Prominent~~ (FIXED)
V1 endpoint pages (route, execute, calldata, health-check) now have Info callouts at the top pointing to their V2 equivalents, plus V2 links in their Related Endpoints sections.

#### 11. No Troubleshooting / Common Issues Page
FAQ covers general questions but there's no dedicated troubleshooting page for common integration issues (slippage errors, failed transactions, timeout handling).

**Recommendation**: Add a troubleshooting page to the API Reference tab.

### Cleanup Priorities (Ordered)

1. **Delete** `api-reference/scroll/` and all Scroll references
2. **Delete** `quickstart.mdx` (Mintlify boilerplate)
3. **Audit & consolidate** `integrate-best-trading/fibrous-api/` (duplicates `api-reference/endpoints/`)
4. **Add to nav or delete** `fibrous-limit-order/` pages
5. **Add to nav or delete** `api-reference/base/`, `api-reference/hyperevm/` chain-specific folders
6. **Complete** widget documentation or remove from nav
7. **Fill in** all TBD values in supported-chains and route docs
8. **Add descriptions** to 14 files missing frontmatter metadata
9. **Update** README.md with Fibrous-specific content
10. **Delete** `developer-resources/` folder (duplicate of contract-addresses in nav)
