---
name: "telemedicine-consultation-broker"
description: "Coordinate secure WebRTC peer connections, ephemeral tokens, and doctor-patient scheduling."
---

# Telemedicine Consultation Broker Skill

## Overview
Manages the orchestration of remote video and audio consultations between patients and licensed healthcare professionals.

## Operations
1. Generates ephemeral, cryptographically signed WebRTC room access tokens with 60-minute TTL.
2. Facilitates ICE candidate and SDP exchange via secure signaling channels.
3. Monitors connection quality and executes dynamic fallback to audio-only mode during bandwidth drops.
4. Enforces immediate token revocation and session teardown upon consultation termination.
