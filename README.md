# Sentinel-Forge

**Forging Elite SOC Capabilities - A 30-Day Cybersecurity Detection Lab**

## Overview

Sentinel-Forge is an advanced 30-day Security Operations Center project that documents the complete build of an ELK-based detection lab. This hands-on journey covers threat simulation, investigation, and response to real-world cyber attacks in a controlled environment.

## Project Description

This repository contains my day-by-day progress through the DFIR 30 Days SOC Challenge, where I built a fully functional security monitoring infrastructure including:

- **SIEM Deployment** (Elastic Stack - Elasticsearch, Kibana, Fleet)
- **Threat Detection** (Custom alerts, dashboards, and detection rules)
- **Incident Response** (Structured investigation workflows)
- **Security Automation** (ELK to OSTicket integration)
- **Endpoint Protection** (EDR implementation with Elastic Defend)

## Lab Architecture

The lab environment was built on Vultr cloud infrastructure and includes:

- **ELK Stack Server** for log aggregation and analysis
- **Fleet Server** for centralized agent management  
- **Windows & Ubuntu Servers** as monitored endpoints
- **Kali Linux** for attack simulation
- **Mythic C2 Framework** for command and control
- **OSTicket** for incident management
- **VPC Network** (172.31.0.0/24) for segmentation

## Progress

✅ **Days 1-10**: Lab setup, ELK deployment, agent configuration  
✅ **Days 11-20**: Attack simulation, detection engineering, visualization  
✅ **Days 21-30**: Incident response, automation, EDR implementation

Each day contains detailed documentation of the tasks completed, configurations applied, and lessons learned.

## Getting Started

To explore this project:

1. Browse the daily folders (Day-01 to Day-30) for step-by-step guides
2. Review the configurations and scripts used throughout the challenge
3. Follow the logical progression from infrastructure setup to advanced detection

## Acknowledgments

This project is based on the [DFIR 30 Days SOC Challenge](https://www.youtube.com/playlist?list=PLtK75QqxTbULze-zk7r8yrfY5p976k7dV) by Ahmed Sajid Butt.

---

*Building detection capabilities, one day at a time.*
