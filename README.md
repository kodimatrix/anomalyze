# Anomalyze (analyze anomaly)

**Anomalyze** is a lightweight Streamlit application that helps you surface *anomalous* vectors—items whose categorical payload (e.g. `"positive"` / `"negative"`) disagrees with the surrounding neighborhood—in a Qdrant collection.
Instead of eyeballing a t-SNE plot, you can:

1. Point the app at your Qdrant cluster (local or remote).
2. Pick which categorical payload key to analyse.
3. Tune sampling size, *k*-nearest-neighbour count, and disagreement threshold.
4. Get an interactive table of “suspicious” points with their neighbours, ready for deep-dives, CSV export, or bulk fixes.

---

## Stack

| Layer                  | Choice        |
| ---------------------- | ------------- |
| **Language / Runtime** | Python 3.12   |
| **Dependency mgr**     | Poetry        |
| **Vector DB client**   | qdrant-client |
| **UI**                 | Streamlit     |
| **Packaging**          | Docker        |

---

## High-level architecture

```
            ┌───────────────────┐
  Streamlit │  UI (sidebar +   │   st.dataframe
  frontend  │  main table)     │◄────────────┐
            └─────────┬────────┘             │
                      │                      │
               Async API calls         Suspicious-
                      │                 point list
            ┌─────────▼────────┐              │
            │  anomalyze.core  │  algorithms  │
            │  • Sampler       │──────────────┘
            │  • k-NN checker  │
            └─────────┬────────┘
                      │
             qdrant-client
                      │
            ┌─────────▼─────────┐
            │   Qdrant cluster  │
            └───────────────────┘
```

---

## Implementation plan / roadmap

| Phase       | Goal                                                                                         |
| ----------- | -------------------------------------------------------------------------------------------- |
| **1**       | Scaffold dependencies, core sampling & neighbor-disagreement algorithm with unit tests       |
| **2**       | MVP UI: connection sidebar, parameter form, results table with neighbor preview & CSV export |
| **3**       | Dockerfile                                                                                   |
| **4**       | CI (GitHub Actions), logging & error handling                                    |
| **5**       | Optional t-SNE scatter, color-coded outliers, deep-link jump                                 |
