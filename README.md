<img width="1292" height="468" alt="image" src="https://github.com/user-attachments/assets/f0340fca-c8cb-4ddb-b928-fb410f9aca83" />


## PD Jewellers
A full-stack MERN e-commerce platform for jewellery business in Sri Lanka. Its novelty feature is a live **3D jewellery configurator** paired with camera-based **AR try-on**.

**Live site:** Coming Soon

---

## Tech Stack

| Layer | Tool | Role |
|---|---|---|
| Frontend | React 18 + TypeScript + Vite | SPA and build tooling |
| Styling | Tailwind CSS v4 | UI styling |
| 3D rendering | React Three Fiber + Three.js | Renders the GLB model in the configurator |
| 3D models | GLB format | Ring and pendant geometry |
| Model optimization | `@gltf-transform/core` (meshopt) | Compresses GLB files before upload |
| AR tracking | MediaPipe | Hand tracking for rings, face/neck for pendants |
| Backend | Node.js + Express + TypeScript | REST API and SPA serving |
| Database | MongoDB Atlas + Mongoose | Products, orders, users, designs |
| Media storage | Cloudinary | Images and GLB (`resource_type: 'raw'`) |
| Email | Nodemailer + Gmail SMTP | Transactional notifications |
| Market data | `xaus.com`, `open.er-api.com` | Gold spot price and USD to LKR rate |
| Export | `jspdf`, `html2canvas-pro` | Dashboard PDF and CSV export |
| Deployment | Render (single Web Service) | Express serves the built Vite SPA, auto-deploy on push to `main` |

---

## Novelty Feature: 3D Configurator + AR Try-On

The whole system connects three pipelines: 3D rendering, live pricing, and AR overlay.

**Flow:**

1. The customer picks metal, stone, shape and size. This updates React state.
2. React Three Fiber renders the GLB model. The metal swap changes PBR material properties (color, metalness, roughness) live, and the size change scales the geometry with a per-size camera FOV adjustment so the size stays visible on screen.
3. At the same time, the live gold rate flows through `PricingContext` and the `price.ts` helpers to show an indicative "Starting from" price that reacts to the metal and stone choice.
4. If the customer switches to AR, the same model is handed to a live camera feed. MediaPipe tracks the hand or neck landmark and overlays the model on it in real time. Size multipliers are widened here so differences read clearly on a phone.
5. The customer can save a screenshot of the try-on (`preserveDrawingBuffer: true`), or add the design to cart / save it.


----------------
<details>
<summary>Click to view screenshots</summary>
<br>

<img width="1312" height="699" alt="image1" src="https://github.com/user-attachments/assets/2c1241b9-0040-4798-84a1-3b343c06c8be" />

<img width="1776" height="837" alt="image2" src="https://github.com/user-attachments/assets/e87c0cba-fa28-4d94-8521-064eea668f84" />

<img width="1752" height="862" alt="image3" src="https://github.com/user-attachments/assets/ad425b96-015c-440c-8d7e-bd175f28bb84" />

<img width="1752" height="862" alt="image3" src="https://github.com/user-attachments/assets/ad425b96-015c-440c-8d7e-bd175f28bb84" />

<img width="1817" height="860" alt="image6" src="https://github.com/user-attachments/assets/b93c161b-10d2-4620-aa3e-e7df32aed3c8" />

<img width="1835" height="856" alt="image5" src="https://github.com/user-attachments/assets/d5548494-dbb1-4369-84be-91b81e1ecb54" />

</details>
----------------------------------------------------------------------------------------------------------------------------------------

**Process diagram:**

```mermaid
flowchart TD
    A[Customer picks metal, stone, shape, size] --> B[React state updates]

    B --> C[React Three Fiber renders GLB model]
    C --> D[Metal swap: PBR color / metalness / roughness]
    C --> E[Size scaling + per-size FOV compensation]

    B --> F[PricingContext + price.ts]
    G[Gold spot API - xaus.com] --> F
    H[USD to LKR - open.er-api.com] --> F
    F --> I[Indicative Starting from price]

    D --> J{Switch to AR?}
    E --> J
    J -->|No| N[Add to cart / save design]
    J -->|Yes| K[Live camera feed]
    K --> L[MediaPipe tracks hand / neck landmark]
    L --> M[Overlay GLB on landmark, widened size multipliers]
    M --> O[Save screenshot - preserveDrawingBuffer]
```
