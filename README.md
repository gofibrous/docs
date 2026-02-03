# Fibrous Finance Documentation

Official documentation for [Fibrous Finance](https://fibrous.finance) - a DEX aggregator providing optimal token swap routing across multiple blockchain networks.

**Live docs**: [docs.fibrous.finance](https://docs.fibrous.finance)

## Supported Networks

- **Starknet** - V1 API + dedicated Starknet endpoints
- **Base** - V1 + V2 API
- **Citrea** - V1 + V2 API
- **HyperEVM** - V1 + V2 API
- **Monad** - V2 API

## Development

Install the [Mintlify CLI](https://www.npmjs.com/package/mintlify) to preview documentation changes locally.

```bash
npm i -g mintlify
```

Run the local dev server at the root of this repository (where `docs.json` is):

```bash
mintlify dev
```

## Publishing

Changes pushed to the default branch are automatically deployed via the Mintlify GitHub app.

## Structure

- `docs.json` - Mintlify configuration (navigation, theme, API settings)
- `essentials/` - Overview pages (FAQ, introduction)
- `api-reference/` - API endpoint documentation (V1, V2, Starknet)
- `fibrous-solutions/` - UI guides (interface, widget)
- `integrate-best-trading/` - SDK reference and smart contracts
- `external-links/` - Links to app, social media, GitHub

## Troubleshooting

- **Mintlify dev not running** - Run `mintlify install` to re-install dependencies
- **Page loads as 404** - Make sure you are running in a folder with `docs.json`
