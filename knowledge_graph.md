# Knowledge Graph - Smart Thermostat Project

This document provides a visual representation of the relationships between different components of the Smart Thermostat project.

## System Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                        Smart Thermostat System                      │
└───────────────────────────────┬─────────────────────────────────────┘
                                │
                ┌───────────────┴───────────────┐
                │                               │
    ┌───────────▼───────────┐     ┌─────────────▼─────────────┐
    │   Frontend (React)    │     │    Backend (Django)       │
    └───────────┬───────────┘     └─────────────┬─────────────┘
                │                               │
    ┌───────────┴───────────┐     ┌─────────────┴─────────────┐
    │                       │     │                           │
┌───▼───┐   ┌───────┐   ┌───▼───┐ │ ┌─────────┐   ┌─────────┐ │
│ Pages │   │ Hooks │   │ Comps │ │ │  API    │   │ Models  │ │
└───┬───┘   └───┬───┘   └───┬───┘ │ └────┬────┘   └────┬────┘ │
    │           │           │     │      │            │      │
    └───────────┴───────────┘     └──────┴────────────┘      │
                                                             │
                                  ┌──────────────────────────┘
                                  │
                        ┌─────────▼──────────┐
                        │ Thermostat Clients │
                        └──────────┬─────────┘
                                   │
                     ┌─────────────┼─────────────┐
                     │             │             │
               ┌─────▼───┐   ┌─────▼───┐   ┌─────▼───┐
               │  Nest   │   │  Cielo  │   │ Pioneer │
               └─────────┘   └─────────┘   └─────────┘
```

## Component Relationships

### Frontend Components

```
┌─────────────────────────────────────────────────────────────────────┐
│                        Frontend Components                          │
└───────────────────────────────┬─────────────────────────────────────┘
                                │
                ┌───────────────┴───────────────┐
                │                               │
    ┌───────────▼───────────┐     ┌─────────────▼─────────────┐
    │        Pages          │     │        Components         │
    └───────────┬───────────┘     └─────────────┬─────────────┘
                │                               │
    ┌───────────┴───────────┐     ┌─────────────┴─────────────┐
    │                       │     │                           │
┌───▼────────┐ ┌──────────┐ │   ┌─▼─────────┐ ┌───────────────▼─┐
│ Dashboard  │ │Properties│ │   │ Layout    │ │ ErrorHandler    │
└────────────┘ └────┬─────┘ │   └───────────┘ └─────────────────┘
                    │       │
            ┌───────┴───────┐     
            │               │     
    ┌───────▼──┐     ┌──────▼───┐ 
    │Thermostats│     │Calendars │ 
    └───────────┘     └──────────┘ 
```

### Backend Components

```
┌─────────────────────────────────────────────────────────────────────┐
│                        Backend Components                           │
└───────────────────────────────┬─────────────────────────────────────┘
                                │
                ┌───────────────┴───────────────┐
                │                               │
    ┌───────────▼───────────┐     ┌─────────────▼─────────────┐
    │        Models         │     │          Views            │
    └───────────┬───────────┘     └─────────────┬─────────────┘
                │                               │
    ┌───────────┴───────────┐     ┌─────────────┴─────────────┐
    │                       │     │                           │
┌───▼────────┐ ┌──────────┐ │   ┌─▼─────────┐ ┌───────────────▼─┐
│ Property   │ │Thermostat│ │   │ Property  │ │ Thermostat      │
└────┬───────┘ └────┬─────┘ │   │ ViewSet   │ │ ViewSet         │
     │              │       │   └───────────┘ └─────────────────┘
     │              │       │
     │       ┌──────┴──┐    │     
     │       │         │    │     
     │ ┌─────▼──┐ ┌────▼───┐ 
     └─┤Calendar│ │Schedule│ 
       └────────┘ └────────┘ 
```

### Thermostat Client Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                    Thermostat Client Architecture                   │
└───────────────────────────────┬─────────────────────────────────────┘
                                │
                        ┌───────▼──────────┐
                        │  BaseClient      │
                        │  (Abstract)      │
                        └───────┬──────────┘
                                │
                     ┌──────────┼──────────┐
                     │          │          │
               ┌─────▼───┐ ┌────▼────┐ ┌───▼─────┐
               │NestClient│ │CieloClient│ │PioneerClient│
               └─────┬───┘ └────┬────┘ └───┬─────┘
                     │          │          │
                     └──────────┼──────────┘
                                │
                        ┌───────▼──────────┐
                        │ ClientFactory    │
                        └───────┬──────────┘
                                │
                        ┌───────▼──────────┐
                        │ ThermostatAPI    │
                        │ Extension        │
                        └──────────────────┘
```

## Data Flow

```
┌──────────┐    ┌───────────┐    ┌────────────┐    ┌─────────────┐
│  User    │───►│  Frontend │───►│  Backend   │───►│ Thermostat  │
│ Interface│    │  (React)  │    │  (Django)  │    │   APIs      │
└──────────┘    └───────────┘    └────────────┘    └─────────────┘
     ▲                ▲                ▲                  │
     │                │                │                  │
     └────────────────┴────────────────┴──────────────────┘
                      Response Flow
```

## Authentication Flow

```
┌──────────┐    ┌───────────┐    ┌────────────┐    ┌─────────────┐
│  User    │───►│  Login    │───►│  Django    │───►│  JWT Token  │
│          │    │  Form     │    │  Auth      │    │  Generated  │
└──────────┘    └───────────┘    └────────────┘    └─────────────┘
                                                          │
┌──────────┐    ┌───────────┐    ┌────────────┐           │
│ Protected│◄───│  Token    │◄───│  Token     │◄──────────┘
│ Resources│    │  Verified │    │  Storage   │
└──────────┘    └───────────┘    └────────────┘
```

## Deployment Flow

```
┌──────────┐    ┌───────────┐    ┌────────────┐    ┌─────────────┐
│  ZIP     │───►│  GitHub   │───►│  Extract   │───►│  Frontend   │
│  Upload  │    │  Action   │    │  ZIP       │    │  Repository │
└──────────┘    └───────────┘    └────────────┘    └─────────────┘
                                       │                  │
                                       │           ┌──────▼──────┐
                                       │           │   Render    │
                                       │           │  Frontend   │
                                       │           └─────────────┘
                                       │
                                 ┌─────▼─────┐    ┌─────────────┐
                                 │  Backend  │───►│   Render    │
                                 │ Repository│    │   Backend   │
                                 └───────────┘    └─────────────┘
```

## Key Dependencies

### Frontend Dependencies
- React
- React Router
- TypeScript
- Vite
- Tailwind CSS

### Backend Dependencies
- Django
- Django REST Framework
- djangorestframework-simplejwt
- Requests (for API calls)
- Gunicorn (for deployment)

## Critical Paths

1. **Authentication Path**: Login → JWT Token → Protected Resources
2. **Thermostat Control Path**: UI → API → Thermostat Client → External API
3. **Deployment Path**: ZIP → GitHub Action → Repositories → Render
4. **Calendar Integration Path**: Property → Calendar → iCal URL → External Calendar

This knowledge graph provides a visual representation of the system architecture and component relationships, helping maintain understanding of the system as a whole across sessions.
