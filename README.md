## Introduction

> **Orbital Pool isn’t just another AMM, it’s a paradigm shift. The future holds a million stablecoins. Today's infrastructure isn't ready.**

Orbital is an automated market maker for pools of 2, 3, or 10,000 stablecoins. It unlocks capital efficiency by bringing concentrated liquidity to higher dimensions.

By bending liquidity into a torus-shaped universe, Orbital unlocks concentrated liquidity for three or more stablecoins at once. This isn’t your usual 2D liquidity grid—it’s an entirely new dimension where LPs can laser-focus around the sacred $1 mark, while still guaranteeing fair prices even if an entire stablecoin implodes to zero.

It’s like Uniswap V3’s surgical precision colliding head-on with Curve’s bulletproof stability, and the result is something that shouldn’t even exist, but somehow, it does.

Orbital is the AMM where capital efficiency doesn’t just scale, it warps!

## Mathematical Visualization
<p float="left"><img src="https://raw.githubusercontent.com/leeederek/sphere-swap/main/media/orbital-gif-1.gif" width="49%" alt="Orbital GIF 1" /> <img src="https://raw.githubusercontent.com/leeederek/sphere-swap/main/media/orbital-gif-2.gif" width="49%" alt="Orbital GIF 2" />
</p>

## Key Features

- **Multi-Token Stability Engine**: Seamlessly trade across three or more stablecoins in a single pool with no more fragmented liquidity.
- **Warped Concentrated Liquidity**: Liquidity providers can laser-focus capital around $1, achieving maximum efficiency while still keeping markets resilient.
- **Torus Invariant Model**: A breakthrough mathematical invariant that curves liquidity across dimensions, ensuring fair pricing even in extreme scenarios.
- **Boundary-Aware Swaps**: Swaps automatically handle transitions between interior and boundary regions, ensuring correct pricing even at the edge of the liquidity surface.
- **Fusion of Giants (Uniswap × Curve)**: Orbital takes Uniswap V3’s precision and Curve’s stability, merging them into a next-generation AMM.
- **Powered by Arbitrum Stylus + Rust**: Our entire mathematical engine runs in Rust via Stylus, unlocking performance and gas efficiency for complex calculations.

## Mathematical Foundation

### The Orbital Model

The Orbital AMM is built on the mathematical foundation described in the [Paradigm Orbital Whitepaper](https://www.paradigm.xyz/2025/06/orbital). The core innovation lies in extending concentrated liquidity to higher dimensions using spherical geometry.

#### Core Mathematical Concepts

**1. Sphere AMM Formula**

<img src="public/orbital_equation.png" width="400" alt="Orbital Equation" /> 

Where:
- `r(vector) = (r, r, ..., r)` vector is the center of the sphere
- `xᵢ` is the AMM's reserve of asset i
- `r` is the radius of the sphere

**2. Torus Invariant**

The pool uses a torus (donut-shaped) invariant that combines:
- **Interior Ticks**: Behave like spheres for normal trading
- **Boundary Ticks**: Behave like circles when reserves hit boundaries

<img src="public/torus_equation.jpg" width="400" alt="Torus Equation" />

**3. Newton's Method**

Implementation of newton's method in Stylus contract to solve torus invariant with multiple fallback strategy.

<img src="public/newton_method.jpg"  alt="Newton" />
<img src="public/torus_invariant.jpg" " alt="Torus"/>

**4. Q96X48 Implementation** 
We utilize the Q96X48 fixed-point arithmetic format, where the lower 48 bits represent the fractional component and the upper 96 bits represent the integer component.

**5. Tick Geometry**

Each tick is defined by:
- **k**: Plane constant (tick identifier)
- **r**: Radius of the tick
- **Status**: Interior or Boundary

## Improvements Made in Open House

### Core Protocol

1. We employ `Q96.48` fixed-point arithmetic (instead of plain `uint256`) to improve precision in price and liquidity computations, especially across ticks.
2. Paradigm’s Orbitals AMM assumes only one liquidity provider (LP) per tick, but our implementation supports multiple LPs per tick, enabling shared exposure at each tick boundary.
3. We support trade segmentation across ticks, i.e, swaps that cross tick boundaries are split and executed part-by-part, matching Paradigm’s trade-segmentation design to ensure correct accounting of liquidity changes and slippage.
4. Liquidity provision now allows liquidity providers to define depeg tolerance, so they can limit exposure to unstable stablecoins during swaps. This gives LPs more control over risk vs returns.

### Expanding Orbital

1. APIs & Documentation: We provide comprehensive developer APIs plus clear documentation, so other projects can integrate seamlessly with our Orbital-like pool mechanics.
2. Agent-to-Agent & x402 Integration: Google’s Agent Payments Protocol (AP2), combined with Coinbase’s x402 extension, currently restricts agents to transact only in USDC. With Orbital, agents can seamlessly swap any supported stablecoin into USDC under the hood and then complete the payment. This means an agent can pay in any stablecoin of their choice, and the recipient still settles in their preferred form, preserving x402’s guarantees while adding flexibility and multi-stablecoin support.

## Contract Addresses

- **Orbital AMM Pool**: `0x8E27C670fA1D45a635e916F8bd60F7E5E1AcF19B`
- **Math Helper (Stylus)**: `0xFD95736D8B046923A954D79Ef4EcDcE047586260`
- **MUSDC-A**: `0x4036B58f91F2A821cB56E2921213663f58db7e6c`
- **MUSDC-B**: `0x41906B6CBFC6a1bEd09311a88e7549a2eB34F325`
- **MUSDC-C**: `0x28f73c76Cb06ceAAA94Adce630f012531f5E80a9`
- **MUSDC-D**: `0x153BD834089ad564fF33450A621EAC412cD4D8f0`
- **MUSDC-E**: `0x987b031Bc36122867108da11686F66D22A9eB460`

## Architecture & User Flow

<img src="public/userflow.png" width="600" alt="UserFlow" /> 

<img src="public/_architecture.png" width="600" alt="Architecture" /> 

## 🚀 Quick Start

### Prerequisites

- Foundry installed
- Node.js 16+ (for testing)
- Git

### Installation

```bash
# Clone the repository
git clone https://github.com/agrawalx/orbital-pool.git
cd orbital-pool

# Install dependencies
forge install

# Build the project
forge build

# Run tests
forge test
```

### Steps to configure your own pool

```bash
# Deploy pool and the mock tokens
forge script script/DeployAndConfig.s.sol --rpc-url $RPC_URL --private-key $PRIVATE_KEY --broadcast

# Command to add liqudity
cast send $POOL_ADDRESS "addLiquidity(uint256,uint256[5])" 3000000000000000 "[1000000000000000000000,1000000000000000000000,1000000000000000000000,1000000000000000000000,1000000000000000000000]" --rpc-url $RPC_URL --private-key $PRIVATE_KEY

# Command to swap 
cast send $POOL_ADDRESS "swap(uint256,uint256,uint256,uint256)" 0 1 20000000000000000000 0 --rpc-url $RPC_URL --private-key $PRIVATE_KEY
```

### Frontend installation

#### Development

```bash
# Install dependencies
npm install

# Start development server
npm run dev
```

#### Environment Variables

Copy `.env.example` to `.env.local` and fill in:

```bash
NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID=your_project_id
```

All contract addresses are pre-configured for Arbitrum Sepolia testnet.

### How to test

1. Run the deployment and the configuration script given in the `script` folder.
  -> navigate to script folder and run `forge script script/DeployAndConfig.s.sol --rpc-url $RPC_URL --private-key $PRIVATE_KEY --broadcast` 
2. Replace the contract addresses with the new addresses in `frontend/src/lib/wallet.ts` and `frontend/src/lib/constants.ts`.
3. Navigate to frontend folder and Run `npm i && npm run dev` and test.

## Youtube Link

- [Video Demo](https://www.youtube.com/watch?v=b1orvJa6OE8)

## Documentation

### Whitepaper Reference

This implementation is based on the [Paradigm Orbital Whitepaper](https://www.paradigm.xyz/2025/06/orbital), which provides the mathematical foundation for the Orbital AMM model.

### Key Concepts from Whitepaper

1. **Sphere AMM**: Base mathematical model using spherical geometry
2. **Torus Invariant**: Combined interior and boundary tick behavior
3. **Tick Consolidation**: Efficient aggregation of similar ticks
4. **Global Trade Invariant**: Method for calculating large trades across multiple ticks

### Development Team

- [@agrawalx](https://github.com/agrawalx)
- [@IshaanXCoder](https://github.com/IshaanXCoder) 
- [@groverInnovate](https://github.com/groverInnovate)
- [@akronim26](https://github.com/akronim26) 

## 📄 License

This project is licensed under the MIT License.

*Built with ❤️ by the Orbital*
