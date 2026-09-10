# Agent Note: Hero composer additive dock

Status: implemented

English | [中文](2026-09-09-hero-composer-additive-dock.zh.md)

## Problem

The resident composer has an additive list below its active-session card, but a blank Session uses the Hero variant of the same InputBar. Reusing the active dock would expose active-session entries in the Hero and would change the meaning of an existing extension point. Replacing the composer or positioning content over it would also give an additive feature control of resident input behavior or geometry.

## Decision

`conversation.hero.composer.dock` is a session-scoped list declared by `conversation.composer.bar`. The resident InputBar renders the list immediately after its card in normal document flow only when its variant is `hero`, its input state exists, and it has a real Session id. The card remains mounted and the list receives no placement or geometry props.

The active `composer` variant continues to render only `conversation.composer.dock`. A missing Session renders neither session-scoped dock. List ordering remains the slot registry's ascending `order` with registration sequence breaking ties.

## Alternatives considered

**Reuse `conversation.composer.dock`.** Rejected because existing occupants target the active composer and would appear in the Hero without opting into that product context.

**Add a placement option to the existing dock.** Rejected because placement would mix two lifecycle contexts into one slot and require every occupant to understand host layout.

**Expose a DOM anchor or composer geometry.** Rejected because an additive entry needs only normal-flow composition; geometry would couple plugins to the resident composer's implementation.

**Replace the Hero composer.** Rejected because the extension content does not own input behavior and must not remove the resident InputBar.

## Consequences

Hero-only features gain an ordered additive location without changing active composer semantics, input behavior, or Session phase logic. Contributors must register separately for Hero and active contexts when they intentionally support both. The slot is unavailable until the composer bar declaration exists and it renders nothing without a real Session.
