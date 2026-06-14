# AI-ENG-R — UI Agents - Browser Control, Desktop Automation & Visual State

The structural integrity of interface-controlling artificial intelligence systems depends on a fundamental law of operating boundaries: user interfaces are partially observable, drift-prone state machines. Traditional automation systems treat the target interface as a static, deterministic layout with permanent locators and predictable transition latencies. This simplified assumptions layer collapses under the execution patterns of production-grade deployments, where client-side state changes, asynchronous networking, dynamic Document Object Model (DOM) mutations, and layout shifts introduce continuous operational divergence.1  
To achieve reliable execution margins, UI agents must be designed as state-verifying interface operators. This architectural model dictates that every interaction—whether page navigation, semantic field inputs, visual targeting, or bulk file transfers—must be verified against the resulting physical, accessibility, and structural state transitions before the control thread executes subsequent operations.4 This report establishes the structural state models, perception pipelines, control-channel architectures, and containment protocols required to deploy resilient and safe UI agents.

## **Doctrinal Foundations: The Interface as a Stateful Object**

A high-dimensional UI agent cannot be built merely by mapping a model's textual outputs directly to keyboard and mouse coordinates.1 When UI control is treated as ungrounded coordinate-emitting actions, systems fail via misplaced events, duplicate form submissions, security-boundary leaks, and infinite failure-recovery loops.1  
The central task of UI agents is to act as state-verifying interface operators. These systems combine DOM, accessibility, screenshot, visual, browser, desktop, and action-history evidence to plan bounded software-interface actions, execute them through sandboxed control channels, verify each resulting UI state transition, and recover safely from interface drift.1

### **The Core Doctrine of Interface Control**

UI agents must treat interfaces as partially observable, drift-prone state machines. Every click, type, navigation, drag, upload, download, and submit action must be grounded in inspected UI state, executed through a controlled channel, and verified against resulting visual, DOM, accessibility, or application state before the agent continues.4  
This principle organizes the inquiry away from simple execution metrics to a continuous verification loop. The useful question is not "Can the agent click the screen?" but "Does the agent know what interface state it is in, what target it intends to manipulate, why that target is correct, what action is permitted, what state should change afterward, whether that change occurred, and what it must do if the interface drifts?".1 Interface agents fail when they confuse dispatched input events with verified task progress.5 A UI action is not complete when the click fires; it is complete when the interface state proves the intended transition occurred.5

### **Taxonomy of Interface Controllers**

To avoid overlapping execution profiles, systems must separate programmatic scripting from model-driven interface agency.1 This comparison details the execution assumptions, failure tolerances, and target interfaces of these controllers:

| System Category | Executive Definition | Execution Assumptions | Failure Tolerances | Primary Interaction Targets |
| :---- | :---- | :---- | :---- | :---- |
| **Browser Automation Script** | A programmatic instruction sequence designed to run a predefined path over a known web page.12 | Assumes static elements, fixed timeouts, and unyielding DOM targets.10 | Zero-tolerance; fails immediately on layout shifts or selector changes.10 | Pre-defined CSS selectors and XPaths.14 |
| **Test Runner** | A scripted execution environment that runs test cases alongside repeatable fixtures and assertions.5 | Assumes isolated local servers, repeatable mock data, and test environments.5 | Fails loudly; prioritizes pinpointing discrepancies over recovery.5 | Explicit test IDs and DOM elements.14 |
| **RPA Workflow** | A robotic process automation macro that automates semi-structured business workflows across legacy interfaces.1 | Assumes stable application windows, consistent screen coordinates, and fixed layouts.1 | Brittle; requires manual re-engineering on minor application updates.1 | screen coordinates, Win32 controls, and legacy properties, Win32 controls, and legacy properties.1 |
| **Browser Agent** | A model-driven agent that uses dynamic planning to operate web interfaces under variable layouts and flows.4 | Operates under dynamic layouts, changing user states, and unannounced redirects.4 | High; handles dynamic layout changes and performs automated recovery.4 | DOM structures, accessibility trees, and visual viewports.4 |
| **Desktop Agent** | A model-driven controller that plans and executes tasks across operating system windows and applications.1 | Operates across local files, multiple native windows, and global system settings.1 | High; manages focus transitions and handles system alerts.1 | Native OS accessibility APIs, system cursors, and display graphics.1 |
| **Computer-Use Agent** | A platform-agnostic agent that interacts with host operating systems via visual screenshots and synthetic inputs.1 | Assumes no direct DOM access, relying on pixel coordinates and screenshots.1 | Variable; handles visual elements but is susceptible to coordinate offsets.1 | Visual viewports and global screen coordinates.1 |

## **The Unified UI State Model**

To operate reliably, the UI agent must maintain a comprehensive, multi-channel representation of the interface. A screenshot is not just an image; it is a rendered snapshot of application state.1 A DOM is not the complete UI; it is a structural representation that may not match rendered truth.10 An accessibility tree may contain semantic controls unavailable from pixels alone.1  
The state representation must capture fifteen distinct dimensions to provide a complete picture of the UI:

| State Dimension | Physical Data Type | Extraction Mechanism | Operational Purpose | Structural Verification Signal |
| :---- | :---- | :---- | :---- | :---- |
| **1. Navigation State** | String (URI / App ID) 18 | browsingContext.get via WebDriver BiDi.18 | Confirms target domain alignment, tracking redirects and browser origins.7 | Matches the active URL path against expected domain patterns.5 |
| **2. Structural Graph** | Hierarchical markup tree 17 | CDP DOM extraction / raw XML traversal.19 | Maps parent-child element nodes and extracts HTML attributes.17 | Confirms that the target element is attached to the active DOM.5 |
| **3. Semantic Substrate** | Accessible role & name graph 8 | Native OS APIs (UIA, AX, AT-SPI2).8 | Resolves element roles, names, states, and descriptions.8 | Validates that roles match active application properties.8 |
| **4. Visual Capture** | viewport screenshot or rendered frame buffer 17 | Page.captureScreenshot / OS screen capture.1 | Analyzes raw pixels to check for visual changes and layout overlays.1 | Confirms rendering contrast and verifies element layouts.17 |
| **5. Spatial Coordinates** | Normalized array: [x1, y1, x2, y2]17 | getBoundingClientRect / AX position query.11 | Coordinates element bounds for visual click fallbacks.1 | Bounding box has a non-zero area and maps inside the viewport.5 |
| **6. Focus Element** | Element reference ID 11 | activeElement traversal via CDP Runtime.13 | Pinpoints the active input cursor location, preventing blind typing.5 | Focus matches the target element's active DOM reference.5 |
| **7. Input Values Map** | Map: Selector -> String 5 | Traverses input element attributes in the DOM.5 | Tracks field text to confirm data entry and clear operations.5 | Input strings match the expected field values.5 |
| **8. Validation Alerts** | Map: Selector -> String 5 | Evaluates aria-invalid attributes and text.5 | Identifies inline input errors and block constraints.5 | Error containers are absent or hidden after correction.5 |
| **9. Modal & Overlay State** | Map: Selector -> BoundingBox | Checks z-index layers and visible overlays.5 | Detects blocking dialogs, consent pop-ups, and screens.5 | Ensures the target element is not obscured by other layers.5 |
| **10. Viewport Offset** | Tuple: (Scroll_x, Scroll_y) 11 | Queries window scroll coordinates.11 | Checks scroll position to confirm element visibility.5 | Target element is positioned within the active viewport bounds.5 |
| **11. Network Traffic** | Integer (active request count) | Monitors requests via WebDriver BiDi.18 | Identifies loading states and pending transactions.5 | active requests settle or an app-specific readiness signal is observed.9 |
| **12. Runtime Console Logs** | List: ConsoleMessage 22 | LogInspector / console listener connections.22 | Catches Javascript exceptions and resource failures.22 | Zero uncaught runtime errors during action execution.22 |
| **13. Session Profile** | Map (cookies and local storage) 18 | storage.getCookies via BiDi.18 | Tracks login states, sessions, and tenant scopes.18 | Active session cookies match target auth configurations.7 |
| **14. Action History** | List: ActionRecord | Internal tracing logs. | Saves the sequence of completed steps to prevent loops. | Verifies execution progress matches the planned path. |
| **15. Risk Profile** | Enum (Low, Medium, High, Critical) | Internal configuration map. | Maps action risks to enforce safety constraints.7 | High-risk actions are gated and require explicit approvals.7 |

## **Dual-Channel UI Perception Model and Fusion Rules**

A robust UI agent cannot rely on a single channel for perception. DOM-only agents fail when elements are visually occluded, offscreen, hidden behind overlays, rendered in canvas, or detached during client-side re-rendering. Screenshot-only agents fail when semantic labels are missing from pixels, small text is unreadable, or keyboard/accessibility relationships are invisible to the visual model.

The agent must combine semantic, structural, visual, spatial, and runtime signals into a unified state model before choosing targets.

```text
+--------------------------------------------------------------------------------
| DUAL-CHANNEL UI PERCEPTION MODEL
+--------------------------------------------------------------------------------
|
| [ Active Interface State ]
|          |
|          +-------------------------------+
|          |                               |
|          v                               v
| [ Semantic / Structural Channel ]   [ Visual / Spatial Channel ]
|   DOM tree                          screenshot / framebuffer
|   accessibility tree                OCR boxes
|   roles and names                   icon detection
|   labels and form relations         visual bounding boxes
|   focus and enabled state           occlusion and contrast
|   selectors and test IDs            viewport coordinates
|          |                               |
|          +---------------+---------------+
|                          |
|                          v
| [ Perception Fusion Engine ]
|   reconcile DOM, AX, screenshot, hit-test, focus, and runtime signals
|                          |
|          +---------------+---------------+
|          |                               |
|          v                               v
| [ Verified Actionable Target ]   [ Conflict / Drift / Ambiguity ]
|   unique target                    re-observe, dismiss overlay,
|   visible                          re-query, inspect visually,
|   enabled                          or escalate
|   stable
|   receives events
|
+--------------------------------------------------------------------------------
| Invariant:
|   A target is actionable only when semantic identity, visual presence,
|   spatial coordinates, and event-receiving state agree.
+--------------------------------------------------------------------------------
```

To resolve structural conflicts, the fusion engine enforces five rules during state construction:

| Rule Category | Conflict Scenario | Detection Diagnostic | Unified Resolving Algorithm | Expected Outcome |
| :---- | :---- | :---- | :---- | :---- |
| **1. Visibility Guard** | DOM element exists but is hidden from the viewport. | DOM visibility, computed style, zero size, offscreen position. | Exclude element from targeting; mark as non-actionable. | Prevents actions on hidden layout elements. |
| **2. Occlusion Verification** | Target coordinates are covered by another element. | Hit-test returns another node; overlay captures pointer events. | Trace z-index and overlay boundaries; route to modal or overlay playbook. | Prevents misclicks into blocking dialogs or banners. |
| **3. Status Cross-Check** | DOM shows enabled, but rendered UI appears disabled. | Pixel grayout, disabled styling, AX disabled state, aria-disabled. | Query DOM, AX, and visual status; require agreement before action. | Prevents clicks on disabled or inert controls. |
| **4. Canvas Resolution** | Control is rendered inside canvas or custom graphics. | DOM exposes canvas or opaque container with no child controls. | Run OCR/icon detection and spatial matching over rendered region. | Produces coordinate target with lower confidence and stricter verification. |
| **5. Accessibility Audit** | DOM element lacks usable labels or stable identifiers. | Missing ID, text, placeholder, aria-label, or test ID. | Query accessibility tree for role, name, description, and relations. | Resolves semantic targets that DOM serialization misses. |

## **Semantic Targeting and Accessibility Tree Models**

UI agents should prefer stable semantic targets over volatile visual coordinates. A button’s accessible role and name usually survive layout shifts better than a pixel coordinate. A form field’s associated label is safer than a nearby text guess. A test ID is safer than a generated CSS class. Visual coordinates remain useful, but they should usually serve as verification and fallback rather than the primary identity of the target.

Windows, macOS, Linux, and browsers expose interface semantics through different APIs. A production UI agent should normalize these channels into a common semantic target object.

```json
{
  "$schema": "https://ai-engineering.canon/schemas/semantic-target-v1.json",
  "target_id": "tgt_2026_06_10_00412",
  "source_context": {
    "context_type": "browser",
    "origin": "https://billing.internal.enterprise",
    "viewport_id": "vp_001",
    "snapshot_id": "snap_2026_06_10_00412"
  },
  "role_descriptor": {
    "standardized_role": "button",
    "html_role": "button",
    "win32_uia_control": "ButtonControlPattern",
    "macos_ax_role": "AXButton",
    "linux_at_spi_role": "ROLE_PUSH_BUTTON"
  },
  "name_descriptor": {
    "accessible_name": "Place order",
    "visible_text": "Place order",
    "placeholder_text": null,
    "aria_label_value": "Submit and finalize your transaction"
  },
  "locators": {
    "test_id_selector": "data-testid=order-checkout-button",
    "role_selector": "getByRole('button', { name: 'Place order' })",
    "label_selector": null,
    "css_selector": "button.btn-submit-order",
    "xpath_selector": "//button[@type='submit' and contains(., 'order')]"
  },
  "spatial_metadata": {
    "bounding_box": [642, 518, 814, 568],
    "coordinate_system": "viewport_css_pixels",
    "device_pixel_ratio": 1.0,
    "viewport_attached": true,
    "center_point": [728, 543]
  },
  "actionability": {
    "is_visible": true,
    "is_stable": true,
    "is_enabled": true,
    "is_editable": false,
    "receives_pointer_events": true,
    "is_occluded": false
  },
  "risk": {
    "risk_classification": "critical_mutation",
    "confirmation_required": true,
    "post_action_verification_required": true
  },
  "targeting_heuristics": {
    "ambiguity_score": 0.0,
    "confidence_rating": 0.985,
    "resolution_channel": "test_id_plus_accessible_name",
    "fallback_channels": [
      "role_name",
      "visual_ocr",
      "spatial_neighborhood"
    ]
  }
}
```

### **Locator Robustness and Selection Strategy**

The targeting engine should evaluate element locators using a reliability hierarchy:

| Locator Strategy | Reliability | Best Use | Failure Mode |
| :--- | :---: | :--- | :--- |
| **Explicit Test IDs** | Highest | Production-owned interfaces, testable workflows, stable internal apps. | Missing if the app was not instrumented for automation. |
| **Accessible Roles and Names** | High | Buttons, links, menus, dialogs, checkboxes, form controls. | Breaks when accessibility metadata is poor or localized unexpectedly. |
| **Associated Form Labels** | High | Text fields, selects, checkboxes, radio groups. | Breaks when labels are missing or visually detached from fields. |
| **Stable Text Content / Placeholder** | Medium | Content pages, search boxes, simple forms. | Sensitive to localization, A/B copy, and dynamic text changes. |
| **DOM Structure / CSS Selector** | Medium-Low | Fallback when semantic locators are unavailable. | Brittle under component refactors and generated class names. |
| **XPath** | Low | Legacy pages or emergency fallback. | Brittle under hierarchy changes. |
| **Visual Coordinate Targeting** | Lowest as identity, useful as fallback | Canvas apps, remote desktops, screenshots, native apps with poor semantics. | Vulnerable to layout shift, scaling, viewport movement, and occlusion. |

Target selection should prefer the strongest available locator but verify the final candidate with visual and actionability checks. A valid locator is not enough if the target is hidden, overlapped, disabled, or outside the viewport.

UI agents should prefer stable semantic targets over volatile visual coordinates to prevent layout shifts or resolution differences from breaking automated workflows.1 By utilizing accessibility names, roles, and structural attributes, agents construct robust targets that survive minor interface changes.8  
Windows, macOS, and Linux manage accessibility trees using distinct, platform-specific APIs.1 To support cross-platform targeting, systems must abstract these variations into a unified semantic target schema:

```JSON  
{  
  "$schema": "https://ai-engineering.canon/schemas/semantic-target-v1.json",  
  "target_id": "tgt_2026_06_10_00412",  
  "role_descriptor": {  
    "standardized_role": "button",  
    "win32_uia_control": "ButtonControlPattern",  
    "macos_ax_role": "AXButton",  
    "linux_at_spi_role": "ROLE_PUSH_BUTTON"  
  },  
  "name_descriptor": {  
    "accessible_name": "Place order",  
    "placeholder_text": null,  
    "aria_label_value": "Submit and finalize your transaction"  
  },  
  "locators": {  
    "css_selector": "button.btn-submit-order",  
    "xpath_selector": "//button[@type='submit' and contains(., 'order')]",  
    "test_id_selector": "data-testid=order-checkout-button"  
  },  
  "spatial_metadata": {  
    "bounding_box": ,  
    "scaling_factor": 1.0,  
    "viewport_attached": true  
  },  
  "actionability": {  
    "is_visible": true,  
    "is_stable": true,  
    "is_enabled": true,  
    "receives_pointer_events": true  
  },  
  "targeting_heuristics": {  
    "ambiguity_score": 0.0,  
    "confidence_rating": 0.985,  
    "risk_classification": "critical_mutation"  
  }  
}
```

### **Locator Robustness and Selection Strategy**

To minimize failures during system updates, the targeting engine evaluates element locators using a strict reliability hierarchy :

1. **Explicit Test IDs (getByTestId)**: The most robust targeting method.14 It relies on dedicated data attributes (data-testid, data-qa) that remain unchanged during layout modifications.14  
2. **Accessible Roles & Names (getByRole)**: Highly resilient.14 It locates elements by how they are perceived by assistive technologies, ensuring the agent interacts with controls identically to human operators.14  
3. **Associated Form Labels (getByLabel)**: The preferred method for form inputs.14 It binds input controls to their visible labels, preventing input misalignment.14  
4. **Text Content & Placeholders (getByText, getByPlaceholder)**: Resilient for content pages.14 However, these are susceptible to localization changes and translation drift.14  
5. **Raw CSS Selectors & XPath (locator)**: Brittle and prone to breakage.15 Changes to utility classes or component hierarchies can disrupt lookups.15 This method is reserved as a fallback of last resort.15

## **Browser Control Architecture**

The browser control layer acts as the physical link between the agent's planning models and the active browser process. Modern high-dimensional systems select from multiple specialized control channels based on the required speed, latency, cross-browser compatibility, and level of introspection needed:

| Feature Dimension | WebDriver Classic | WebDriver BiDi | Chrome DevTools Protocol | Native Accessibility Bridge |
| :---- | :---- | :---- | :---- | :---- |
| **Transport Layer** | HTTP REST requests (stateless).12 | Persistent bidirectional WebSocket connection.18 | Persistent bidirectional WebSocket connection.19 | Native OS IPC and COM/CoreFoundation calls.8 |
| **Command Response Latency** | High; introduces network overhead per HTTP call.26 | Low; bidirectional websocket messaging.26 | Minimal; direct socket communication with browser process.26 | Single-digit millisecond-level native API calls.4 |
| **Event Streaming** | Lacks streaming; requires continuous HTTP polling.28 | Native streaming of lifecycle, console, and network events.18 | Native streaming of DOM, network, and security events.13 | Native OS event notifications (observers).8 |
| **Execution Performance** | Slower; execution requires driver translation.12 | Extremely fast; direct browser protocol execution.29 | Fast; removes protocol translations.26 | Extremely fast; executes directly on background windows.4 |
| **Introspection Depth** | High-level; limited to user actions and DOM states.12 | Mid-level; captures console, script contexts, and requests.18 | Complete low-level access (memory, profiler, network).13 | Visual controls, roles, positions, and element hierarchies.1 |
| **Cross-Browser Support** | Universal standard supported by all vendors.12 | Emerging standard; supported by major browsers.12 | Proprietary; limited to Chromium-based browsers.29 | Platform-specific; separate implementations required per OS.2 |
| **Context Isolation** | Managed via driver browser parameters. | Dedicated user contexts and sandboxed evaluations.18 | Supports multiple targets and isolated page sessions.13 | Window-level isolation; tracks Frontmost focus.16 |

## **Desktop Automation Boundary Model**

Operating outside the browser sandbox introduces critical safety concerns. A desktop automation agent may interact with local applications, filesystems, native dialogs, terminals, credential prompts, system settings, and global clipboard state. The system must therefore isolate execution, restrict host access, filter network egress, and verify native UI state before dispatching input events.

```text
+--------------------------------------------------------------------------------
| DESKTOP AUTOMATION BOUNDARY MODEL
+--------------------------------------------------------------------------------
|
| [ User / Orchestrator ]
|          |
|          v
| [ Policy and Permission Gate ]
|   allowed apps | allowed files | allowed domains | action risk class
|          |
|          v
| [ Isolated Execution Environment ]
|   VM / container / remote desktop / disposable user profile
|          |
|          +--> [ Filesystem Boundary ]
|          |      read/write workspace only
|          |      no ambient host home-directory access
|          |
|          +--> [ Credential Boundary ]
|          |      brokered credentials
|          |      no raw secrets in model context
|          |
|          +--> [ Network Boundary ]
|          |      egress allowlist
|          |      logging and block rules
|          |
|          +--> [ Clipboard Boundary ]
|          |      isolated clipboard
|          |      no host clipboard leakage
|          |
|          v
| [ Native UI Bridge ]
|   Windows UIA | macOS AX | Linux AT-SPI2 | screenshot fallback
|          |
|          v
| [ Action Dispatcher ]
|   click | type | shortcut | drag | file picker | app command
|          |
|          v
| [ Post-Action Verification ]
|   focus | window state | file state | app state | visual state
|
+--------------------------------------------------------------------------------
| Invariant:
|   Desktop control must never inherit ambient host authority.
|   It operates inside a bounded, observable, revocable environment.
+--------------------------------------------------------------------------------
```

To coordinate interaction across operating systems, desktop bridges use native APIs that present distinct structural features:

| Platform API | Win32 UI Automation (UIA) | macOS AXUIElement API | Linux AT-SPI2 Bridge |
| :---- | :---- | :---- | :---- |
| **Interface Technology** | COM interface via UI Automation client APIs. | CoreFoundation / AXUIElement interfaces. | DBus-based messaging layer over accessibility bus. |
| **Role Categorization** | Strongly typed roles mapping to fixed enums. | Role and subrole string classifications. | Standardized roles with custom application exposure. |
| **Query Mechanism** | Supports cache requests and bulk property fetching. | Element-by-element calls, with batching possible for attributes. | Element-by-element DBus messaging. |
| **Performance Profile** | Strong when cache requests are used. | Moderate; depends on batching and app responsiveness. | Can be slow due to DBus round trips. |
| **Query Optimization** | Cache filters and scoped tree queries. | Multi-attribute copy operations and subtree pruning. | Lazy evaluation and targeted role queries. |
| **Element Verification** | Validates roles, patterns, properties, and bounding rectangles. | Verifies supported actions such as press, focus, or value set. | Verifies available action strings and state sets. |
| **Stable Reference** | Cached snapshots can stale after UI changes. | AX references can stale after window/app updates. | Node changes require re-querying. |
| **Execution Tooling** | UIA clients, automation bridges, remote desktop controllers. | AX clients, app-specific scripting, remote desktop controllers. | AT-SPI2 clients and desktop automation bridges. |

Desktop automation is inherently higher-risk than browser-only automation because the control surface is broader. The default posture should be disposable session, least privilege, blocked host leakage, and strict post-action verification.

## **UI Action Taxonomy and Bounded Planning**

To simplify action planning, UI interactions are categorized into an exhaustive taxonomy of five operations. Actions must be explicitly bound to target elements and state preconditions:

| Action Category | Action Identifier | Input Parameters | Expected Interface Change | Precondition Criteria |
| :---- | :---- | :---- | :---- | :---- |
| **Observation** | ObserveState 1 | Viewport target.1 | Refreshes the active UI state model.4 | Target page context is active.18 |
|  | InspectElement 8 | Target element locator.14 | Captures structural attributes and coordinates.8 | Element node is attached to the DOM.5 |
| **Navigation** | NavigateTo 18 | Destination URL.18 | Target URL loads; page enters network idle.5 | Destination matches domain policies.7 |
|  | NavigateBack 18 | None. | Browser context rolls back to prior history state. | Browser history list contains past pages. |
|  | RefreshPage 18 | None. | Document reloads; active DOM cache is cleared.2 | Target page context is active.18 |
| **Interaction** | Click 11 | Target locator, position offset.11 | Dispatches pointer event; triggers layout change.5 | Element is visible, stable, and enabled.5 |
|  | DoubleClick 11 | Target locator, position offset.11 | Dispatches double-click sequence to target.11 | Element is visible, stable, and enabled.5 |
|  | Hover 35 | Target locator, hover delay.35 | Dispatches pointer moves; triggers tooltip rendering.5 | Target element is inside the viewport.5 |
|  | Type 35 | Target locator, character string.35 | Appends string values to target input.5 | Field is focused and editable.5 |
|  | ClearValue 11 | Target locator.11 | Selects and deletes active input strings.11 | Field is focused and editable.5 |
|  | SelectValue 21 | Target locator, option ID.21 | Selected property updates inside select node.5 | Select element is enabled and not read-only.5 |
|  | CheckControl 11 | Target checkbox locator.11 | Check state updates; aria-checked set to true.5 | Element is an unchecked checkbox.5 |
|  | DragAndDrop | Source locator, destination coordinates. | Source element is moved to target area. | Source and target elements are attached. |
| **System** | Upload 18 | Target input, file path.18 | File properties are updated inside upload node.5 | Target file exists in the sandbox.34 |
|  | Download 18 | Target action locator.18 | File is saved to the designated download path.18 | Host path is write-enabled and has space.34 |
|  | CopyText | Target source locator. | String is copied to the isolated sandbox clipboard. | Source element contains selectable text.5 |
|  | PasteText | Target destination locator. | Clipboard contents are pasted into input.5 | Destination input field is focused.5 |
|  | KeyboardShortcut 1 | Keyboard character combination.1 | System event is triggered inside active context.16 | Context window is active and focused.16 |

### **The Bounded Action Planning Model**

To prevent unverified execution paths, the planning engine evaluates every UI operation as a bounded action record. The record defines target identity, preconditions, execution payload, risk controls, post-action expectations, timeout policy, and recovery playbook.

```json
{
  "plan_record_id": "act_2026_06_10_00412",
  "target_domain": "billing.internal.enterprise",
  "active_action_type": "Click",
  "risk_class": "critical_mutation",
  "target": {
    "semantic_target_id": "tgt_confirm_payment",
    "preferred_locator": "getByRole('button', { name: 'Confirm payment' })",
    "fallback_locator": "data-testid=btn_confirm_payment",
    "expected_role": "button",
    "expected_name": "Confirm payment",
    "bounding_box": [642, 518, 814, 568],
    "coordinate_system": "viewport_css_pixels"
  },
  "preconditions": [
    "target.is_visible == true",
    "target.is_stable == true",
    "target.is_enabled == true",
    "target.receives_pointer_events == true",
    "target.is_occluded == false",
    "active_origin == 'https://billing.internal.enterprise'",
    "payload_hash == approved_payload_hash"
  ],
  "execution_payload": {
    "dispatch_method": "CDP:Input.dispatchMouseEvent",
    "click_point": [728, 543],
    "button": "left",
    "click_count": 1
  },
  "confirmation": {
    "required": true,
    "approval_id": "appr_2026_06_10_7781",
    "approval_scope": "single_execution",
    "expires_at": "2026-06-10T18:35:00Z"
  },
  "post_action_expectations": [
    "payment_request_submitted == true",
    "network_request_contains_idempotency_key == true",
    "success_or_pending_status_visible == true",
    "duplicate_warning_absent == true",
    "action_ledger_entry_created == true"
  ],
  "verification_policy": {
    "timeout_ms": 10000,
    "readiness_signal": "network_quiet_or_app_specific_completion",
    "requires_backend_verification": true,
    "requires_visual_verification": true
  },
  "error_recovery_playbook": "PLY_RECV_CLICK_NO_EFFECT",
  "stop_conditions": [
    "target_ambiguous",
    "approval_expired",
    "payload_mismatch",
    "cross_origin_redirect",
    "duplicate_transaction_warning"
  ]
}
```

A bounded action may dispatch only after all preconditions pass. If a precondition fails, the agent must re-observe, repair, ask for confirmation, or escalate. It must not improvise a nearby coordinate and hope the browser is feeling generous.

## **Click and Type Verification Framework**

The Click and Type Verification Framework acts as the system's execution-level gate. It ensures that inputs are verified directly against resulting UI state transitions before subsequent actions are permitted.

```
+---------------------------------------------------------------------------------------------------------+  
|                                    CLICK & TYPE VERIFICATION TIMELINE                                   |  
+---------------------------------------------------------------------------------------------------------+  
|                                                                                                         |  
|   0ms                     20ms                    100ms                   250ms                         |  
|   +-----------------------+-----------------------+-----------------------+-------------------------+   |  
|   | Pre-Action Check      | Event Dispatch        | Settle Delay          | Post-Action Verification|   |  
|   |                       |                       |                       |                         |   |  
|   | - Confirms visibility | - Programmatic click  | - Monitors page load  | - Audits DOM mutations  |   |  
|   | - Evaluates stability |   or key sequences    | - Tracks transitions  | - Checks input values   |   |  
|   | - Verifies enablement |   dispatched over     | - Waits for active    | - Confirms redirects    |   |  
|   | - Checks occlusion    |   active socket       |   network idle state  | - Validates toast text  |   |  
|   +-----------------------+-----------------------+-----------------------+-------------------------+   |  
|                                                                                                         |  
+---------------------------------------------------------------------------------------------------------+
```

To coordinate interaction across different UI elements, the verification pipeline executes across four sequential stages:

| Verification Phase | Pre-Action Checks | Action Dispatch Method | Settle & Sleep Checks | Post-Action Verification |
| :---- | :---- | :---- | :---- | :---- |
| **Click Verification** | Confirms element visibility, stability, and enablement. Verifies target is not obscured by overlay elements. | Dispatches pointer clicks via CDP input events or invokes platform AXPress. | Monitors CSS transitions and waits for animation frames to stabilize. | Verifies navigation changes, target element detachment, or success toast rendering. |
| **Type Verification** | Confirms field visibility, focus, and editable properties. | Sends character sequences via CDP insert text commands. | Waits for input events to trigger and monitors active client-side validators. | Audits resulting input values against expected strings. |
| **Select Verification** | Validates target select element and active dropdown choices. | Dispatches option selection events or updates selected DOM attributes. | Monitors select container for list collapses or overlay dismissals. | Verifies active option properties match targeted indices. |
| **Check Verification** | Validates checkbox elements and active check parameters. | Dispatches pointer events to checkboxes or updates checking properties. | Waits for toggle transitions to complete. | Evaluates resulting checkbox attributes (aria-checked="true"). |
| **Drag Verification** | Identifies source and destination coordinates. | Sends programmatic mouse-down, drag-path, and mouse-up events. | Waits for layout containers to adapt to element changes. | Verifies target element coordinate intersections inside destination boundaries. |
| **Upload Verification** | Confirms target input element has valid file upload types. | Dispatches file paths directly to target inputs via automation APIs. | Waits for processing and monitors form status. | Verifies uploaded file properties are recognized by the DOM tree. |
| **Download Verify** | Confirms download action controls are attached and enabled. | Dispatches pointer events to download triggers. | Monitors browser context for file download events. | Verifies downloaded files are written to the sandbox directory. |
| **Navigate Verify** | Confirms destination URLs match allowed domains. | Sends navigation requests over active browser connections. | Waits for page load completions and monitors redirects. | Checks resulting URL properties and audits console states. |
| **Submit Verification** | Executes Form Submission Safety checklist. | Sends click events to form submit buttons. | Waits for transitions and verifies active database state updates. | Confirms navigation changes or checks backend API records. |

### **Mitigating the React Re-render Race Condition**

A common automation failure occurs when React unmounts and replaces a DOM element between the agent's pre-action check and event dispatch.10 While the element remains visually identical, holding an outdated DOM reference throws an Element is not attached to the DOM exception 10:  
Race Window (T_race) = T_dispatch - T_check < 50ms  
This race window is typically under 50 milliseconds. The race condition is mitigated using two runtime design patterns:

* **Dynamic Re-Querying**: The execution engine must never store raw element references. Every action must accept a dynamic locator (e.g., page.getByRole("button", { name: "Place order" })) that re-queries and resolves the element target immediately before dispatching the event.10  
* **Web-First Retrying Assertions**: Verification loops must avoid immediate value checks (heading.textContent()) that fail on dynamic page loads.38 Instead, the system must utilize auto-retrying, polling assertions (e.g., expect(heading).toHaveText("Order completed")) that repeatedly evaluate criteria over a 5-second window to accommodate rendering delays.6

## **Form Submission Safety Model**

Form submissions are critical action boundaries because they often trigger durable side effects: database writes, purchases, message sends, file uploads, account changes, legal acknowledgments, or financial transactions. A UI agent must treat a form submit as a tool-like mutation, not as “just another click.”

```text
+--------------------------------------------------------------------------------
| FORM SUBMISSION SAFETY MODEL
+--------------------------------------------------------------------------------
|
| [ Draft Form State ]
|          |
|          v
| [ Field Verification ]
|   required fields | labels | values | masks | validation errors
|          |
|          v
| [ Origin and Frame Verification ]
|   allowed domain | frame ancestry | HTTPS | tenant/session scope
|          |
|          v
| [ Payload Compilation ]
|   normalized payload | target endpoint | recipient | amount | file list
|          |
|          v
| [ Duplicate and Idempotency Check ]
|   action history | idempotency key | prior submissions | warning banners
|          |
|          v
| [ Risk Evaluation ]
|   low | medium | high | critical
|          |
|          +--> low risk
|          |       submit after verification
|          |
|          +--> medium / high / critical
|                  render payload review
|                  require approval or out-of-band confirmation
|                  submit only if payload and approval match
|          |
|          v
| [ Submit Action ]
|   click | keyboard submit | programmatic form submit if allowed
|          |
|          v
| [ Post-Submit Verification ]
|   DOM state | network response | app status | backend/API/ledger when available
|
+--------------------------------------------------------------------------------
| Rule:
|   A form submit is complete only when the resulting state is verified.
|   A success toast is evidence, not proof.
+--------------------------------------------------------------------------------
```

To coordinate forms across processing states, the validation engine maps actions to distinct submission stages:

| Submission Stage | Required Verification Controls | Core Processing Algorithm | Expected Verification Signals | Exception Mitigation Protocol |
| :---- | :---- | :---- | :---- | :---- |
| **1. Draft** | Confirm active form, labels, focus order, and field editability. | Inspect form structure and bind labels to inputs. | Form container attached, visible, and scoped to expected origin. | Re-observe form or halt if structure diverges. |
| **2. Field Entry** | Verify field values, masks, dropdown states, checkboxes, and file attachments. | Compare normalized values against planned payload. | Input values match expected values after client-side formatting. | Clear and re-enter values; preserve validation errors. |
| **3. Ready** | Check required fields, inline errors, disabled submit state, and hidden required controls. | Evaluate DOM, accessibility state, and visible validation text. | Submit control is enabled; validation errors absent. | Resolve missing fields or route to repair. |
| **4. Payload Review** | Compile target recipient, amount, destination, endpoint, files, terms, and side-effect class. | Hash payload and compare against intended action. | Payload hash matches plan and user-visible summary. | Halt if payload differs from plan. |
| **5. Duplicate Guard** | Check action history, idempotency key, warning banners, and prior backend state. | Compare active payload to previous submissions. | No duplicate warning; no prior identical terminal transaction unless idempotent. | Cancel or escalate on duplicate risk. |
| **6. Confirmation** | Apply risk-based approval policy. | Require lightweight, explicit, strict readback, or dual-control confirmation. | Valid approval token bound to payload hash. | Halt if approval missing, expired, or mismatched. |
| **7. Submitted** | Dispatch submit action through verified target. | Click verified submit control or invoke allowed submit mechanism. | Submit event fires; UI enters loading/pending state. | Do not retry blindly; inspect for no-effect or duplicate state. |
| **8. Accepted** | Observe request acceptance or application acknowledgment. | Monitor network response, page transition, or server acknowledgment. | HTTP success, accepted status, or pending transaction ID. | Treat accepted as pending, not completed. |
| **9. Successful** | Verify durable state change. | Check DOM, app-specific state, backend/API/ledger where available. | Success state satisfies expected predicates. | Commit task progress only after verification. |
| **10. Failed** | Extract inline, modal, network, or server-side error. | Normalize failure into repairable/non-repairable category. | Error text, failed response, validation state, or unchanged backend state. | Repair, replan, or escalate. |
| **11. Unknown** | Execution result cannot be verified. | Preserve trace, payload hash, and possible submission state. | Timeout, lost connection, ambiguous UI, or unavailable backend. | Block duplicate submit until reconciliation. |

The submit button is not a magic portal to truth. It is a dangerous little rectangle and should be treated accordingly.

## **Browser Sandboxing, Permissions, and Containment**

When interacting with untrusted websites, the agent's browser runtime must be isolated to prevent credential theft, data leaks, and local process exploits.36 Modern configurations harden browser environments using process-level operating system sandboxes and remote browser isolation frameworks:

| Isolation Domain | Risk Surface | Programmatic Defense Configuration | Intended Containment Boundary |
| :---- | :---- | :---- | :---- |
| **Ephemeral Profiles** | Extends active session history, exposing cookie stores and local caches. | Launches browser using incognito context parameters (--incognito). | Wipes all browser data, cookies, and cache upon context teardown. |
| **Credential Masking** | Exposes passwords and tokens to the agent's prompt context. | Implements secure, headless credential brokers to inject inputs directly into the page. | Prevents credential leaks into context windows or logging outputs. |
| **Password Isolation** | Exposes local keychain vaults and system password managers. | Disables password autofill capabilities (--disable-save-password-bubble). | Blocks access to host credential vaults and system resources. |
| **Download Control** | Triggers drive-by malware downloads or writes to host filesystems. | Restricts download pathways to temporary sandboxed directories (/tmp/downloads). | Blocks execution of downloaded files on the host computer. |
| **Upload Restrictions** | Allows unauthorized exfiltration of sensitive host files. | Restricts upload access to a single, read-only workspace directory. | Prevents the agent from reading or uploading private host data. |
| **Clipboard Isolation** | Leaks sensitive clipboard data or system credentials. | Restricts sandbox browser clipboard APIs and access controls. | Prevents data exchange between host and sandbox clipboards. |
| **Extension Shield** | Vulnerable browser extensions intercept traffic and leak credentials. | Disables extensions on startup (--disable-extensions). | Limits execution to verified browser core components. |
| **Egress Filtering** | Initiates requests to malicious domains or exfiltrates data. | Implements container-level egress filters using domain whitelists. | Restricts outbound traffic to approved API and service endpoints. |
| **Permission Controls** | Accesses cameras, microphones, or location coordinates. | Rejects permission prompts automatically via browser preferences. | Prevents unauthorized monitoring and tracking. |
| **Pop-up Blocking** | Spawns multiple windows to bypass isolation boundaries. | Enforces single-tab restrictions and blocks window creation. | Restricts execution to the active, monitored browser tab. |
| **Cross-Origin Framing** | Frame injection attacks bypass security controls. | Enforces Site Isolation policies, running frames in separate processes. | Prevents malicious frames from accessing parent context data. |
| **Context Teardown** | Orphaned processes leak memory and retain session files. | Closes active connections and wipes temporary mount spaces. | Reclaims system resources and ensures zero persistent traces. |

## **Interface Drift Detection and Recovery**

Interface drift is a primary cause of automation failures. Layout shifts, A/B tests, localization changes, modal dialogs, session expiration, client-side re-rendering, and selector changes can all invalidate a previously valid plan.

The central recovery principle is:

**When the UI changes, the plan is stale. Re-observe before acting.**

```text
+--------------------------------------------------------------------------------
| INTERFACE DRIFT RECOVERY PROTOCOL
+--------------------------------------------------------------------------------
|
| [ Planned Next Action ]
|          |
|          v
| [ Pre-Action Verification Fails ]
|   missing target | ambiguity | occlusion | stale selector | unexpected page
|          |
|          v
| [ Pause Execution ]
|   stop dispatching input events
|   preserve current trace and action history
|          |
|          v
| [ Re-Observe Interface State ]
|   DOM | AX tree | screenshot | URL | focus | network | overlays
|          |
|          v
| [ Classify Drift Variant ]
|   selector | layout | modal | session | localization | network | A/B
|          |
|          v
| [ Recovery Attempt ]
|   re-query locator | dismiss overlay | wait | scroll | replan | ask user
|          |
|          +--> target resolved and verified
|          |       update plan and resume
|          |
|          +--> target unresolved or unsafe
|                  halt, escalate, or transfer control
|
+--------------------------------------------------------------------------------
```

To coordinate drift recovery, the system executes a structured Interface Drift Recovery Model:

| Drift Variant | Visual/DOM Root Cause | Detection Diagnostic Signal | First-Line Automated Recovery | Fallback Resolution Channel | Stop Condition Threshold |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **1. Stale Selector** | Dynamic class names or element hierarchy changed. | Locator fails to resolve in active DOM. | Re-query using accessible role/name and test IDs. | Visual coordinate matching with OCR/neighborhood check. | Timeout or 3 failed query attempts. |
| **2. Layout Shift** | Responsive styling, ads, lazy loading, or animation moves target. | Bounding box changes during actionability checks. | Wait for element position to stabilize. | Recalculate viewport-relative coordinates. | Target remains unstable after 2 seconds. |
| **3. A/B Variant** | Workflow structure or route changes unexpectedly. | URL is plausible but page structure diverges. | Re-evaluate visible workflow elements. | Replan from current state. | Replanning loop fails. |
| **4. Localization Shift** | Labels change language or copy. | Role structure exists but expected text missing. | Use test IDs, roles, and layout relations. | Resolve by structural placement and visual text. | Confidence below threshold. |
| **5. Responsive Hide** | Menus collapse on narrow/mobile viewport. | Element exists but is hidden/offscreen. | Resize, scroll, or open menu container. | Switch to mobile-specific workflow. | Target remains invisible. |
| **6. Dynamic Rearrange** | Search/table rows reorder during execution. | Row coordinates shift or unique key moves. | Locate row by stable cell key. | Filter/search table to isolate target row. | Target key missing. |
| **7. Cookie Banner** | Consent overlay blocks interaction. | Hit-test returns overlay element. | Find and dismiss allowed banner control. | Ask user or apply site policy. | Overlay persists after attempts. |
| **8. Modal Block** | Dialog intercepts viewport controls. | Active modal layer or z-index overlay. | Close/cancel if safe; otherwise ask user. | Escalate if modal is security/payment/permission related. | Modal remains or action risk rises. |
| **9. Progress Spinner** | Async requests delay content. | Spinner or loading state remains active. | Wait using readiness policy. | Refresh or re-observe after timeout. | Spinner exceeds timeout. |
| **10. Session Expired** | Redirect to login or auth page. | URL/cookies indicate auth failure. | Use approved credential broker or ask user. | Suspend workflow. | Login loops or MFA required. |
| **11. Input Mask Shift** | JS formats typed values. | Value verification mismatch. | Clear and retype using slower path or direct value set if safe. | Ask user or use alternate input mode. | Diverges after retries. |
| **12. Multi-Label Match** | Multiple identical controls. | Locator returns multiple nodes. | Scope by parent container and nearby labels. | Human review or visual selection. | Ambiguity remains after filters. |

## **UI Recovery Playbook Library**

The UI Recovery Playbook Library provides developers with predefined, deterministic routines to handle common automation exceptions and maintain workflow stability 4:

| Playbook Identifier | Exception Trigger Case | Action Sequence Strategy | Target Recover State | Escalation / Stop Conditions |
| :---- | :---- | :---- | :---- | :---- |
| **PLY_RECV_MISSING** | Target element is missing or selector fails to resolve.5 | 1. Pauses action loop for 1.5s.1 2. Clears element caches.2 3. Re-queries using accessible names.10 | Element successfully resolved and verified as active. | Aborts and escalates if element is missing after 3 attempts.4 |
| **PLY_RECV_AMBIGUOUS** | Locator resolves to multiple matching DOM elements.14 | 1. Filters results using visibility checks.14 2. Evaluates adjacent labels.6 3. Selects based on spatial coordinates.1 | Target is uniquely identified and isolated. | Immediately halts if targeting ambiguity remains.14 |
| **PLY_RECV_NO_EFFECT** | Click action succeeds but triggers no state changes.5 | 1. Retries click using visual coordinates.1 2. Attempts focus and sends Enter key inputs.5 3. Clears browser focus state.5 | Page state transition or visual mutation verified.5 | Aborts after 2 failed interaction retries. |
| **PLY_RECV_BAD_PAGE** | Navigation loads an unexpected page or error screen.5 | 1. Clears navigation context.18 2. Rewinds page history.18 3. Re-dispatches navigation requests.13 | Active context successfully loads target URL. | Halts if navigation fails twice.10 |
| **PLY_RECV_NAV_TIME** | Page navigation times out or network requests hang.5 | 1. Cancels pending requests.18 2. Refreshes active tab context.18 3. Evaluates current DOM structures.1 | Page settles on a usable, functional state. | Aborts if page fails to load within 30 seconds.10 |
| **PLY_RECV_SPINNER** | Dynamic page content is blocked by loading spinners.1 | 1. Polls loading spinner element status.9 2. Waits up to 10s for spinner unmount.9 3. Refreshes page if hang persists.18 | Progress indicators disappear; page content loads. | Escalates to user if spinner persists after page refresh.7 |
| **PLY_RECV_VALIDATE** | Input action triggers an inline validation error.5 | 1. Clears input field values.11 2. Re-types inputs slowly to trigger events.1 3. Dispatches focus blur events.5 | Validation markers are resolved and cleared. | Halts if inputs repeatedly trigger validation errors. |
| **PLY_RECV_LOGIN** | Page session expires and redirects to login.7 | 1. Identifies login form selectors. 2. Requests credentials from secure vault.7 3. Re-submits login form.7 | Target page session is restored and verified.7 | Aborts if login actions loop back to authentication screen.47 |
| **PLY_RECV_CAPTCHA** | Automation is blocked by a CAPTCHA verification prompt. | 1. Halts execution immediately.40 2. Saves the active session trace.1 3. Escalates control to user.7 | User resolves verification; session context restored. | Instantly halts and transfers control; no retries.40 |
| **PLY_RECV_PERM** | Context navigation triggers a browser permission prompt. | 1. Intercepts prompt using BiDi events.18 2. Rejects request following safety profile.18 3. Closes prompt window.18 | Context permission prompt is cleared and dismissed. | Rejects request and blocks camera/location access.46 |
| **PLY_RECV_PICKER** | Action triggers an unannounced native file picker. | 1. Scans context for active file picker events.18 2. Sets targets using file path controls.5 3. Dismisses native window.18 | File path successfully linked to upload control. | Aborts if native file dialog locks execution focus. |
| **PLY_RECV_DL_BLOCKED** | File download action is blocked by sandbox policy.34 | 1. Audits file metadata and extension types.18 2. Confirms file path matches safety scopes.34 3. Clears blocks if within bounds.18 | Download succeeds and writes to sandboxed directory. | Halts if downloaded file type violates safety limits.34 |
| **PLY_RECV_UP_FAIL** | File upload action fails or is rejected by form. | 1. Verifies local file exists and is readable.34 2. Validates format against form attributes.5 3. Re-dispatches upload path.5 | Form accepts file; upload progress finishes.5 | Aborts if form repeatedly rejects file format.5 |
| **PLY_RECV_OVERLAY** | Target click coordinate is blocked by an overlay.5 | 1. Identifies the overlay's close control.5 2. Sends clicks to dismiss pop-up. 3. Re-evaluates target actionability.5 | Overlay dismissed; target receives pointer events.5 | Aborts if overlay remains visible after 2 attempts. |
| **PLY_RECV_CRASH** | Page process crashes, displaying an error screen.46 | 1. Captures process crash dump logs.22 2. Re-creates the page context.13 3. Re-runs step sequence from history state. | Page context is restored and verified as functional. | Aborts if page crashes repeatedly during steps.46 |
| **PLY_RECV_NETWORK** | Requests fail due to local connection drops. | 1. Pauses execution and checks connection status. 2. Waits up to 10s for network to restore. 3. Retries failed navigation step.18 | Connection is restored; target page loads successfully. | Aborts if connection remains offline after 3 attempts. |
| **PLY_RECV_REDIRECT** | Navigation triggers unexpected cross-origin redirect.5 | 1. Audits target domain origin safety.7 2. Blocks inputs on unverified domains.7 3. Returns to prior history state.18 | Context is returned to a verified page domain. | Aborts if redirected domain violates safety policies.34 |
| **PLY_RECV_EXPIRED** | User session expires during workflow execution.47 | 1. Suspends active step sequence.4 2. Authenticates session via secure login broker.7 3. Restores execution context. | Active session is restored; target workflow resumes. | Halts if authentication attempts fail.47 |
| **PLY_RECV_SUB_ERR** | Form submission returns a server-side error.7 | 1. Extracts error text from target containers.5 2. Updates input fields with corrected data.11 3. Re-runs Form Submission safety checks.7 | Form accepts corrected payload; success confirmed.7 | Aborts if server returns repeatable 500 error codes. |
| **PLY_RECV_DUP_WARN** | Form returns duplicate transaction warning.7 | 1. Scans page for warning confirmation prompts.5 2. Verifies backend transaction logs.7 3. Cancels submit to prevent duplicates.7 | Submission is safely cancelled; status updated. | Halts and escalates to user for validation.7 |
| **PLY_RECV_CONFIRM** | Risky action triggers a confirmation dialog.7 | 1. Compares payload with planned variables.7 2. Scans for confirm button elements.5 3. Clicks to confirm if values match plan.5 | Dialog is dismissed; target mutation proceeds.5 | Halts and escalates if payload values diverge.7 |

## **UI Prompt Injection and Interface Instruction Collision**

UI agents read dynamic, untrusted page content: comments, ads, help text, documents, emails, chat messages, form labels, OCR text, hidden DOM nodes, and user-generated content. This creates a critical vulnerability: indirect prompt injection, where attacker-controlled interface content attempts to override the agent’s instructions, exfiltrate data, or trigger unauthorized actions.

The core security rule is:

**Page content is evidence, not authority.**

```text
+--------------------------------------------------------------------------------
| INDIRECT PROMPT INJECTION LIFECYCLE
+--------------------------------------------------------------------------------
|
| [ Attacker-Controlled UI Content ]
|   visible text | hidden DOM | comments | SVG | ads | documents | OCR text
|          |
|          v
| [ Extraction / Serialization ]
|   DOM text | accessibility tree | OCR | screenshot captions | page summary
|          |
|          v
| [ Context Assembly Risk ]
|   untrusted data placed near instructions or tool descriptions
|          |
|          v
| [ Instruction Collision ]
|   model may confuse page text with developer/system authority
|          |
|          +--> unsafe design
|          |       tool misuse | data exfiltration | credential leak | unauthorized action
|          |
|          +--> safe design
|                  data is delimited, sanitized, policy-gated,
|                  and blocked from granting tool authority
|
+--------------------------------------------------------------------------------
| Invariant:
|   Untrusted UI text can describe the world.
|   It cannot grant permissions, redefine goals, or authorize tools.
+--------------------------------------------------------------------------------
```

Attack delivery vectors:

| Attack Delivery Vector | Programmatic Concealment Method | Parser Extraction Vulnerability | Core System Security Guard |
| :---- | :---- | :---- | :---- |
| **1. Zero-Sizing** | Text hidden with `font-size: 0`, zero width/height, or offscreen positioning. | DOM extractors capture hidden strings; visual models miss them. | Filter by computed visibility, size, viewport intersection, and role relevance. |
| **2. CSS Hidden Divs** | `display: none`, `visibility: hidden`, opacity, clipping, or transform tricks. | Raw DOM serialization may include invisible instructions. | Exclude non-visible content unless specifically requested as code/data evidence. |
| **3. HTML Comments** | Payload placed inside `<!-- malicious instruction -->`. | Naive HTML-to-text pipelines may serialize comments. | Strip comments before model context unless auditing source markup. |
| **4. SVG / XML / CDATA** | Payload embedded inside SVG, XML metadata, or CDATA. | OCR or XML parsers may extract instructions without provenance. | Sanitize vector assets and mark extracted text as untrusted data. |
| **5. Semantic Blend** | Malicious instruction woven into normal page copy. | Model cannot separate content from authority by text alone. | Role separation, data delimiters, and tool-policy enforcement outside the model. |
| **6. Multilingual Override** | Same instruction repeated across languages. | Single-language filters miss alternate phrasing. | Multilingual classification and invariant policy enforcement. |
| **7. Serialization Breakout** | Payload contains strings such as `</untrusted-data>` or JSON/XML delimiters. | Context wrapper can be broken if text is not escaped. | Escape delimiters and serialize untrusted content as data, never raw control markup. |
| **8. Visual Prompt Injection** | Instructions rendered in image, screenshot, ad, or document. | OCR text enters prompt as if it were user/developer instruction. | Treat OCR output as evidence with source label and zero authority. |

### **Principles of System-Data Separation**

| Control | Purpose |
| :--- | :--- |
| **Role Separation** | System/developer instructions, user goals, page content, and tool results must remain distinct channels. |
| **Delimited Evidence Blocks** | Untrusted UI content should be wrapped as data with source labels and escaped syntax. |
| **Capability Sandboxing** | A read-only browsing task should not possess write, email, filesystem, or payment tools. |
| **Policy Enforcement Outside the Model** | Tool permissions, egress controls, credential access, and filesystem access must be enforced by runtime policy. |
| **Computed Visibility Filtering** | Hidden page text should not silently become instruction context. |
| **Source-Aware Summarization** | Summaries should retain whether content came from page text, OCR, metadata, or user instruction. |
| **Action Gating** | Page content cannot authorize actions; only user/system policy and verified approvals can. |
| **Injection Telemetry** | Suspected prompt injection payloads should be logged with source region and blocked capability path. |

A model trained to “ignore malicious instructions” is helpful. It is not a security boundary. Runtime authority boundaries are the boundary. The model is the intern with a clipboard; the sandbox is the locked door.

## **Deceptive Interface Risk Model**

UI agents can be tricked by deceptive design patterns, phishing pages, lookalike domains, fake browser chrome, clickjacking layers, hidden costs, and visual spoofing. A state-verifying UI agent must distinguish between genuine platform state and webpage-rendered imitation.

```text
+--------------------------------------------------------------------------------
| DECEPTIVE INTERFACE RISK MODEL
+--------------------------------------------------------------------------------
|
| [ Candidate Interface Action ]
|          |
|          v
| [ Origin and Transport Verification ]
|   URL | scheme | certificate | frame ancestry | redirect chain
|          |
|          v
| [ Native-vs-Web Prompt Classification ]
|   browser/OS dialog or webpage imitation?
|          |
|          v
| [ Semantic-Visual Consistency Check ]
|   DOM role | AX role | screenshot | hit-test | visible labels
|          |
|          v
| [ Interaction Safety Check ]
|   occlusion | clickjacking | hidden charges | prechecked options
|          |
|          v
| [ Execution Policy ]
|   allow | require confirmation | block | escalate
|
+--------------------------------------------------------------------------------
```

To secure agents against deceptive designs, developers must implement a multi-layered verification model:

| Threat Category | Spoofing Mechanism | Primary Detection Method | Core System Mitigation |
| :---- | :---- | :---- | :---- |
| **Phishing Pages** | Lookalike login screens steal credentials. | Verify active URL, domain allowlist, certificate, frame ancestry, and redirect chain. | Block credential broker on unverified origins. |
| **Clickjacking** | Transparent overlays intercept legitimate clicks. | Hit-test target coordinates and compare event receiver with intended target. | Reject clicks when another element receives pointer events. |
| **Hidden Costs** | Prechecked options, fees, or terms are visually minimized. | Scan form state, checkboxes, fine print, totals, and changed values. | Surface payload review and require approval for cost changes. |
| **Spoofed Native Prompts** | Webpage imitates OS/browser dialogs or security warnings. | Compare active dialog to native window handles / browser prompt APIs. | Ignore webpage-rendered fake system prompts; route real native prompts through permission policy. |
| **Malicious Downloads** | Drive-by file downloads or disguised executable files. | Monitor browser download events and file metadata. | Restrict downloads to sandbox; block execution and unsafe extensions. |
| **Credential Harvest Forms** | Page asks for passwords, tokens, MFA codes, or API keys outside approved flow. | Compare form origin and field purpose to credential policy. | Use credential broker only on allowlisted domains and workflows. |
| **Domain Confusion** | Unicode/homograph, typo-squat, or embedded third-party frame. | Normalize domain, inspect eTLD+1, detect punycode and frame origin mismatch. | Block or require explicit user confirmation. |
| **Visual Misdirection** | Buttons or labels visually imply one action while DOM submits another. | Compare visible text, form action, network endpoint, and click target. | Halt on mismatch and require review. |
| **Dark Pattern Confirmation** | UI nudges agent into unwanted add-ons, subscriptions, or consent. | Detect prechecked options, hidden recurring terms, and opt-out language. | Default to conservative choices and present user review. |

The agent should never enter credentials, approve payments, accept terms, download files, or submit forms on a page whose origin, prompt type, and payload semantics have not been verified.

## **UI Agent Evaluation and Observability Guidance**

Evaluating and debugging UI agents is difficult because interfaces are dynamic, nondeterministic, and partially observable. Task success rate alone is insufficient. A system can complete a task while misclicking, leaking credentials, relying on brittle selectors, ignoring prompt injection, or failing to verify the final state.

Production observability must track perception, targeting, execution, verification, safety, recovery, and replay.

```text
+--------------------------------------------------------------------------------
| UI AGENT OBSERVABILITY ARCHITECTURE
+--------------------------------------------------------------------------------
|
| [ Agent Run Trace ]
|          |
|          +--> [ Perception Artifacts ]
|          |      screenshots | DOM snapshots | AX tree | OCR boxes | focus state
|          |
|          +--> [ Targeting Artifacts ]
|          |      locator attempts | bounding boxes | hit-test results | ambiguity
|          |
|          +--> [ Execution Artifacts ]
|          |      clicks | typing | navigation | uploads | downloads | shortcuts
|          |
|          +--> [ Verification Artifacts ]
|          |      post-action DOM | visual diff | network result | app state | backend check
|          |
|          +--> [ Safety Artifacts ]
|          |      sandbox blocks | egress blocks | prompt-injection detections | approvals
|          |
|          +--> [ Replay Package ]
|                 ordered snapshots | action records | timing | browser profile | artifacts
|
+--------------------------------------------------------------------------------
| Invariant:
|   A UI-agent trace should let an auditor replay what the agent saw,
|   what it targeted, what it did, and how it knew the action worked.
+--------------------------------------------------------------------------------
```

Metrics:

| Metric Category | Technical Metric Identifier | Diagnostic Target | Standard Production Target |
| :---- | :---- | :---- | :---- |
| **Perception** | UI OCR Character Error Rate | Accuracy of text extraction from rendered interfaces. | Task-specific threshold; lower for financial/legal fields. |
| **Perception** | Bounding Box IoU Accuracy | Precision of spatial element localization and grounding. | IoU >= 0.85 on target controls. |
| **Perception** | DOM-Screenshot Agreement | Agreement between DOM-visible text and rendered screenshot text. | > 98% on visible text nodes in controlled apps. |
| **Perception** | Accessibility Coverage Rate | Fraction of actionable controls with usable roles and names. | High for controlled apps; alert on regression. |
| **Targeting** | Grounding Hit Rate | Accuracy of selecting intended UI targets. | > 95% target resolution rate. |
| **Targeting** | Locator Resolution Failure Rate | Frequency of failed or ambiguous locator resolution. | < 2% of active execution steps where locators are expected. |
| **Targeting** | Locator Resolution Latency | Overhead from dynamic re-querying. | Kept within step latency budget. |
| **Targeting** | Occlusion Block Rate | Actions blocked because target is covered. | Nonzero is acceptable; spikes indicate UI drift or modal issues. |
| **Execution** | Step Success Rate | Percentage of interface actions completed without recovery. | > 98% per individual workflow step in stable apps. |
| **Execution** | Misclick Rate | Clicks whose event receiver differs from intended target. | Near zero. |
| **Execution** | Type Fidelity Rate | Inputs where final field values match expected strings. | 100% for controlled strings. |
| **Execution** | Action Retry Rate | Frequency of retries per action type. | Low and bounded; alert on loops. |
| **Verification** | State-Transition Match Rate | Post-action state satisfies expected predicates. | > 95%, task dependent. |
| **Verification** | Focus Verification Rate | Field focus confirmed before typing. | 100% of typing steps. |
| **Verification** | Form Submission Verification Rate | Submitted forms verified through UI/network/backend signal. | 100% for high-impact submits. |
| **Verification** | Unknown-State Rate | Actions where result could not be verified. | Must be low; high-impact unknown states require escalation. |
| **Safety** | Sandbox Isolation Violation Attempts | Attempts to access forbidden host resources. | Zero successful violations; attempted blocks should be investigated. |
| **Safety** | Egress Policy Block Count | Attempts to access unapproved domains. | Blocks may indicate controls working; alert on unexpected spikes. |
| **Safety** | Credential Exposure Events | Secrets exposed to model context, logs, screenshots, or page text. | Zero. |
| **Safety** | Prompt Injection Detection Rate | Suspected page instruction attacks. | Tracked; spikes trigger source review. |
| **Recovery** | Drift Recovery Success Rate | Automated recovery from layout, selector, or modal drift. | High on known apps; bounded attempts. |
| **Recovery** | Human Escalation Rate | Runs requiring user/operator intervention. | Context dependent; spikes indicate automation fragility. |
| **Replay** | Trace Completeness Rate | Runs with screenshots, DOM/AX state, action records, and verification artifacts. | 100% for production actions. |
| **Replay** | Deterministic Replay Success | Ability to reconstruct action sequence from stored artifacts. | Required for regulated or high-impact workflows. |
| **Telemetry** | Average Task Duration | End-to-end time to complete workflow. | Task-specific; monitor regressions. |

Evaluation harnesses should include:

| Test Class | Purpose |
| :--- | :--- |
| **Locator Drift Tests** | Verify target selection under class/name/layout changes. |
| **Occlusion Tests** | Confirm the agent blocks clicks under overlays. |
| **Re-render Race Tests** | Verify stale references are re-queried before dispatch. |
| **Prompt Injection Tests** | Ensure page content cannot grant tool authority. |
| **Sandbox Escape Tests** | Confirm filesystem, clipboard, credential, and egress boundaries. |
| **Form Submit Tests** | Validate duplicate prevention, confirmation, and post-submit verification. |
| **Replay Tests** | Confirm traces reconstruct perception, action, and verification. |

## **Cross-Canon Handoff Map**

The systems architecture of AI-ENG-R integrates with adjacent disciplines across the AI Engineering Systems Canon. Its durable handoff is verified interface state: what the agent saw, what it targeted, what it did, what changed, what failed, and what evidence proves the UI transition.

| Target Canon Report | Domain Area | Technical Handoff Parameters | Engineering Integration Rules |
| :---- | :---- | :---- | :---- |
| **AI-ENG-B** | Context Tenure & State Governance | UI snapshots, browser profile state, cookies, local storage, action history. | Enforce retention, redaction, and cleanup rules for session artifacts and browser state. |
| **AI-ENG-C** | Cost, Latency & Margin Management | Step timeouts, rendering latency, locator retries, browser/session costs. | Coordinate action budgets with network readiness, rendering stability, and recovery loops. |
| **AI-ENG-L** | Serving Architecture | Stateful browser/desktop sessions, remote browser pools, backpressure controls. | Coordinate browser and desktop sessions with low-latency serving pools, queues, and capacity limits. |
| **AI-ENG-M** | Agentic Orchestration | UI action loops, stop states, retry budgets, escalation states. | Enforce bounded action sequences and terminate loops on repeated drift or ambiguity. |
| **AI-ENG-N** | Tool Contracts | Browser/desktop action schemas, dispatch payloads, side-effect class. | Treat UI actions as tool contracts with validation, authorization, idempotency, and traceability. |
| **AI-ENG-O** | Action Verification | Post-action UI state, backend/API verification, unknown-state handling. | Dispatch is not completion; state transition verification gates subsequent actions. |
| **AI-ENG-P** | Multimodal Understanding | UI element coordinates, screenshots, crops, OCR regions, visual grounding. | Use visual segmentation and OCR for fallback targeting and evidence citation. |
| **AI-ENG-Q** | Speech and Realtime Interaction | Voice-driven UI commands, spoken confirmations, transcript-to-target mapping. | Bind voice-derived actions to finalized transcript, UI target verification, and confirmation policy. |
| **AI-ENG-S** | Production Pathologies | Layout drift, stale selectors, browser crashes, prompt injection, recovery loops. | Monitor UI-specific failure modes and route recurring issues to incident/problem management. |
| **AI-ENG-T** | Prompt Injection and Trust Boundaries | Untrusted page text, hidden DOM, OCR instructions, data/authority separation. | Treat UI content as evidence, not instructions; enforce tool authority outside the model. |
| **AI-ENG-U** | Dependency Risk | Browser engines, automation protocols, OCR/VLM parsers, accessibility bridges. | Track protocol compatibility, parser drift, and sandbox dependency health. |
| **AI-ENG-V** | Resource Protection | Navigation loops, download abuse, egress blocks, sandbox violations. | Limit action volumes, domain access, file access, downloads, and recursive browsing. |
| **AI-ENG-W** | Fallback and Degraded Modes | DOM failure, visual fallback, human handoff, read-only degraded mode. | Activate visual coordinate matching, text-only fallback, or manual control when UI perception degrades. |
| **AI-ENG-X** | User Trust and Control | Action preview, visible target boxes, confirmation cards, progress display. | Let users inspect, approve, interrupt, and understand interface actions. |
| **AI-ENG-Y** | Human Review | Escalation packets, screenshots, target candidates, ambiguity records. | Transfer session context to users/operators on unresolved targeting or high-risk confirmation. |
| **AI-ENG-Z** | Telemetry and Metrics | UI traces, action spans, screenshots, DOM/AX snapshots, verification results. | Export standardized telemetry for perception, targeting, execution, verification, safety, and replay. |
| **AI-ENG-AA** | Evaluation | UI task benchmarks, drift tests, sandbox tests, injection tests. | Evaluate agents against web, desktop, visual targeting, and safety regression suites. |
| **AI-ENG-AB** | Audit and Replay | Signed traces, screenshots, DOM/AX snapshots, action records, verification artifacts. | Store replayable UI sessions for debugging, audits, and incident review. |
| **AI-ENG-AC** | Incident Response | Sandbox quarantine, credential exposure, malicious downloads, unsafe submits. | Isolate compromised sessions, revoke credentials, preserve evidence, and trigger UI-agent incident playbooks. |
| **AI-ENG-AD** | Governance, Policy, Compliance & Accountability | Approval rules, credential-use policy, data retention, regulated UI workflows. | Govern high-impact UI actions, credential handling, audit obligations, and accountability boundaries. |
| **AI-ENG-AJ** | Reference Architecture | Remote browser isolation, desktop sandbox clusters, UI state model, recovery library. | Provide implementation blueprints for production UI-agent platforms. |

The durable handoff is this:

```text
AI-ENG-R exports verified interface action state:
observed UI, selected target, dispatched action, resulting transition,
verification evidence, recovery path, and replay artifacts.
```

## **Durable Principles of UI Agents**

1. **Interfaces are State Machines**  
   A user interface is a partially observable, drift-prone state machine. Clicks, typing, uploads, downloads, and submits are hypotheses that must be verified against resulting visual, semantic, structural, application, or backend state before execution can proceed.

2. **Dispatch Is Not Completion**  
   A click event, keyboard event, or navigation request is not task progress by itself. Progress is established only when the expected interface transition has been verified.

3. **Dynamic Locators Are Mandatory**  
   Static element references stale under client-side rendering. Agents must re-query dynamic locators immediately before dispatch and verify actionability at the moment of execution.

4. **Semantic Targets Beat Coordinates Until They Don’t**  
   Roles, names, labels, test IDs, and accessibility structures should be preferred over raw coordinates. Coordinates remain necessary for canvas, remote desktop, screenshots, and visual fallback, but they require stricter verification.

5. **Visual State and Structural State Must Reconcile**  
   DOM truth, accessibility truth, and screenshot truth can diverge. Robust UI agents fuse all available channels and halt when the target is ambiguous, hidden, occluded, stale, or visually inconsistent.

6. **Forms Are Tool-Like Mutation Boundaries**  
   Form submissions can send money, messages, files, credentials, legal approvals, or irreversible account changes. They require payload review, duplicate prevention, approval policy, and post-submit verification proportional to risk.

7. **Untrusted UI Text Is Data, Not Authority**  
   Page content, comments, ads, OCR text, emails, documents, and hidden DOM nodes may contain hostile instructions. They can inform the task, but they cannot redefine policy, grant permissions, or authorize tools.

8. **Containment Is the Default Runtime Posture**  
   UI agents should run in disposable, isolated browser or desktop environments with restricted filesystem, clipboard, credential, permission, and network access. No agent should inherit the user’s ambient host authority.

9. **Recovery Must Be Bounded**  
   Interface drift, missing elements, modals, and stale selectors should trigger deterministic recovery playbooks with attempt limits. Infinite “try another click” loops are not resilience. They are chaos in a trench coat.

10. **High-Risk Actions Require Human-Visible Review**  
   Payments, submissions, deletes, credential operations, external messages, and regulated workflows require payload review and explicit approval when policy demands it.

11. **Every Production UI Action Should Be Replayable**  
   A production trace should preserve screenshots, DOM/AX snapshots, locators, bounding boxes, dispatched actions, timing, verification signals, recovery decisions, and final state.

12. **The Agent Controls the Interface; Policy Controls the Agent**  
   The model may plan and propose. The runtime validates, authorizes, dispatches, verifies, contains, logs, and stops. Authority lives in the system boundary, not in whatever the webpage whispered into the prompt.00

#### **Works cited**

1. Desktop Automation AI Agents: Beyond the Browser | by Oktay Ateş | May, 2026 | Medium, accessed June 10, 2026, [https://medium.com/@oktayates/desktop-automation-ai-agents-beyond-the-browser-031c52b0ec7a](https://medium.com/@oktayates/desktop-automation-ai-agents-beyond-the-browser-031c52b0ec7a)  
2. I built a library that gives AI agents structured UI access via accessibility APIs, like Playwright but for the entire OS - Reddit, accessed June 10, 2026, [https://www.reddit.com/r/AI_Agents/comments/1s8o7gs/i_built_a_library_that_gives_ai_agents_structured/](https://www.reddit.com/r/AI_Agents/comments/1s8o7gs/i_built_a_library_that_gives_ai_agents_structured/)  
3. Learn to Adapt: Robust Drift Detection in Security Domain - arXiv, accessed June 10, 2026, [https://arxiv.org/pdf/2206.07581](https://arxiv.org/pdf/2206.07581)  
4. computer-use: overhaul for improved agentic desktop + browser interaction · Issue #4445 · daytonaio/daytona - GitHub, accessed June 10, 2026, [https://github.com/daytonaio/daytona/issues/4445](https://github.com/daytonaio/daytona/issues/4445)  
5. Auto-waiting - Playwright, accessed June 10, 2026, [https://playwright.dev/docs/actionability](https://playwright.dev/docs/actionability)  
6. Best Practices - Playwright, accessed June 10, 2026, [https://playwright.dev/docs/best-practices](https://playwright.dev/docs/best-practices)  
7. Indirect prompt injection through enterprise data is becoming a real attack surface - Reddit, accessed June 10, 2026, [https://www.reddit.com/r/aiagents/comments/1tgjzfj/indirect_prompt_injection_through_enterprise_data/](https://www.reddit.com/r/aiagents/comments/1tgjzfj/indirect_prompt_injection_through_enterprise_data/)  
8. Cross-platform desktop automation through accessibility APIs ..., accessed June 10, 2026, [https://crowecawcaw.github.io/general/2026/05/30/accessibility-for-computer-use.html](https://crowecawcaw.github.io/general/2026/05/30/accessibility-for-computer-use.html)  
9. Mastering waits and timeouts in Playwright - CircleCI, accessed June 10, 2026, [https://circleci.com/blog/mastering-waits-and-timeouts-in-playwright/](https://circleci.com/blog/mastering-waits-and-timeouts-in-playwright/)  
10. Playwright auto-wait is great, until your component re-renders mid-action | Mergify, accessed June 10, 2026, [https://mergify.com/blog/playwright-auto-wait-element-rerenders](https://mergify.com/blog/playwright-auto-wait-element-rerenders)  
11. Locator - Playwright, accessed June 10, 2026, [https://playwright.dev/docs/api/class-locator](https://playwright.dev/docs/api/class-locator)  
12. Automation Protocols - WebdriverIO, accessed June 10, 2026, [https://webdriver.io/docs/automationProtocols/](https://webdriver.io/docs/automationProtocols/)  
13. CDP vs. BiDi: Browser Automation Protocol Internals for Scrapers - Evomi Blog, accessed June 10, 2026, [https://evomi.com/blog/cdp-vs.-bidi-browser-automation-protocol-internals-for-scrapers](https://evomi.com/blog/cdp-vs.-bidi-browser-automation-protocol-internals-for-scrapers)  
14. Locators - Playwright, accessed June 10, 2026, [https://playwright.dev/docs/locators](https://playwright.dev/docs/locators)  
15. React Playwright Testing: Complete Guide - Autonoma AI, accessed June 10, 2026, [https://getautonoma.com/blog/react-playwright-testing-guide](https://getautonoma.com/blog/react-playwright-testing-guide)  
16. Browser Control Layer: Programmatic Desktop Automation via macOS Accessibility API for AI Agent Swarms - Pubroot, accessed June 10, 2026, [https://pubroot.com/ai/agent-architecture/browser-control-layer-programmatic-desktop-automation-via-macos-accessib/](https://pubroot.com/ai/agent-architecture/browser-control-layer-programmatic-desktop-automation-via-macos-accessib/)  
17. AI-ENG-P — Multimodal Understanding - Documents, Images, Tables, Charts & Video  
18. WebDriver BiDi reference - MDN Web Docs, accessed June 10, 2026, [https://developer.mozilla.org/en-US/docs/Web/WebDriver/Reference/BiDi](https://developer.mozilla.org/en-US/docs/Web/WebDriver/Reference/BiDi)  
19. Chrome DevTools Protocol - GitHub Pages, accessed June 10, 2026, [https://chromedevtools.github.io/devtools-protocol/](https://chromedevtools.github.io/devtools-protocol/)  
20. Revisiting WebDriver and Selenium: Understanding History and Internals (CDP/BiDi) - Zenn, accessed June 10, 2026, [https://zenn.dev/mima_ita/articles/61c12757495cc0?locale=en](https://zenn.dev/mima_ita/articles/61c12757495cc0?locale=en)  
21. axuiautomation package - github.com/tmc/apple/x/axuiautomation - Go Packages, accessed June 10, 2026, [https://pkg.go.dev/github.com/tmc/apple/x/axuiautomation](https://pkg.go.dev/github.com/tmc/apple/x/axuiautomation)  
22. WebDriver BiDi: 2023 status update | Blog - Chrome for Developers, accessed June 10, 2026, [https://developer.chrome.com/blog/webdriver-bidi-2023](https://developer.chrome.com/blog/webdriver-bidi-2023)  
23. Selenium Chrome DevTools Protocol (CDP) API: How Does It Work? - Applitools, accessed June 10, 2026, [https://applitools.com/blog/selenium-chrome-devtools-protocol-cdp-how-does-it-work/](https://applitools.com/blog/selenium-chrome-devtools-protocol-cdp-how-does-it-work/)  
24. Prompt injection in AI agents: are your controls keeping up?, accessed June 10, 2026, [https://nhimg.org/community/agentic-ai-and-nhis/prompt-injection-in-ai-agents-are-your-controls-keeping-up/](https://nhimg.org/community/agentic-ai-and-nhis/prompt-injection-in-ai-agents-are-your-controls-keeping-up/)  
25. Trust Playwright's Auto-Waiting for End-to-End Testing - YouTube, accessed June 10, 2026, [https://www.youtube.com/shorts/eDdsynE9Xw0](https://www.youtube.com/shorts/eDdsynE9Xw0)  
26. WebDriver vs Chrome DevTools (Speed Test) - Abstracta, accessed June 10, 2026, [https://abstracta.us/blog/testing-tools/webdriver-vs-chrome-devtools-speed-test/](https://abstracta.us/blog/testing-tools/webdriver-vs-chrome-devtools-speed-test/)  
27. WebDriver BiDi - W3C, accessed June 10, 2026, [https://www.w3.org/TR/webdriver-bidi/](https://www.w3.org/TR/webdriver-bidi/)  
28. Chrome DevTools Protocol (CDP) - Pydoll, accessed June 10, 2026, [https://pydoll.tech/docs/deep-dive/fundamentals/cdp/](https://pydoll.tech/docs/deep-dive/fundamentals/cdp/)  
29. WebDriver BiDi - The future of cross-browser automation | Blog - Chrome for Developers, accessed June 10, 2026, [https://developer.chrome.com/blog/webdriver-bidi](https://developer.chrome.com/blog/webdriver-bidi)  
30. BiDirectional functionality - Selenium, accessed June 10, 2026, [https://www.selenium.dev/documentation/webdriver/bidi/](https://www.selenium.dev/documentation/webdriver/bidi/)  
31. IUIAutomationElement7::FindAllWithOptionsBuildCache (uiautomationclient.h) - Win32 apps, accessed June 10, 2026, [https://learn.microsoft.com/en-us/windows/win32/api/uiautomationclient/nf-uiautomationclient-iuiautomationelement7-findallwithoptionsbuildcache](https://learn.microsoft.com/en-us/windows/win32/api/uiautomationclient/nf-uiautomationclient-iuiautomationelement7-findallwithoptionsbuildcache)  
32. IUIAutomationCacheRequest (uiautomationclient.h) - Win32 apps - Microsoft Learn, accessed June 10, 2026, [https://learn.microsoft.com/en-us/windows/win32/api/uiautomationclient/nn-uiautomationclient-iuiautomationcacherequest](https://learn.microsoft.com/en-us/windows/win32/api/uiautomationclient/nn-uiautomationclient-iuiautomationcacherequest)  
33. Caching UI Automation Properties and Control Patterns - GitHub, accessed June 10, 2026, [https://github.com/MicrosoftDocs/win32/blob/docs/desktop-src/WinAuto/uiauto-cachingforclients.md](https://github.com/MicrosoftDocs/win32/blob/docs/desktop-src/WinAuto/uiauto-cachingforclients.md)  
34. AI Agent Sandbox: How to Safely Run Autonomous Agents in 2026 - Firecrawl, accessed June 10, 2026, [https://www.firecrawl.dev/blog/ai-agent-sandbox](https://www.firecrawl.dev/blog/ai-agent-sandbox)  
35. Locator | Playwright Python, accessed June 10, 2026, [https://playwright.dev/python/docs/api/class-locator](https://playwright.dev/python/docs/api/class-locator)  
36. What Is Remote Browser Isolation (RBI) and How Does It Work? - NordLayer, accessed June 10, 2026, [https://nordlayer.com/learn/browser-security/what-is-browser-isolation/](https://nordlayer.com/learn/browser-security/what-is-browser-isolation/)  
37. DevTools Event Listener, accessed June 10, 2026, [https://chromium.googlesource.com/chromium/src/+/refs/heads/main/chrome/test/chromedriver/docs/event_listener.md](https://chromium.googlesource.com/chromium/src/+/refs/heads/main/chrome/test/chromedriver/docs/event_listener.md)  
38. Playwright Assertions: Avoid Race Conditions with This Simple Fix! - DEV Community, accessed June 10, 2026, [https://dev.to/playwright/playwright-assertions-avoid-race-conditions-with-this-simple-fix-dm1](https://dev.to/playwright/playwright-assertions-avoid-race-conditions-with-this-simple-fix-dm1)  
39. Playwright Assertions: Avoid Race Conditions with This Simple Fix! - YouTube, accessed June 10, 2026, [https://www.youtube.com/watch?v=1VxkHP8vfGg](https://www.youtube.com/watch?v=1VxkHP8vfGg)  
40. Fooling AI Agents: Web-Based Indirect Prompt Injection Observed in ..., accessed June 10, 2026, [https://unit42.paloaltonetworks.com/ai-agent-prompt-injection/](https://unit42.paloaltonetworks.com/ai-agent-prompt-injection/)  
41. Why Your AI Agents Need a Sandbox: OpenSandbox by Alibaba Explained - Emelia.io, accessed June 10, 2026, [https://emelia.io/hub/opensandbox-ai-agents-guide](https://emelia.io/hub/opensandbox-ai-agents-guide)  
42. Indirect Prompt Injection in Web-Browsing Agents | Promptfoo, accessed June 10, 2026, [https://www.promptfoo.dev/blog/indirect-prompt-injection-web-agents/](https://www.promptfoo.dev/blog/indirect-prompt-injection-web-agents/)  
43. Sandbox - Chromium Docs, accessed June 10, 2026, [https://chromium.googlesource.com/chromium/src/+/HEAD/docs/design/sandbox.md](https://chromium.googlesource.com/chromium/src/+/HEAD/docs/design/sandbox.md)  
44. Site Isolation - The Chromium Projects, accessed June 10, 2026, [https://www.chromium.org/Home/chromium-security/site-isolation/](https://www.chromium.org/Home/chromium-security/site-isolation/)  
45. bureado/awesome-agent-runtime-security - GitHub, accessed June 10, 2026, [https://github.com/bureado/awesome-agent-runtime-security](https://github.com/bureado/awesome-agent-runtime-security)  
46. chrome://sandbox Diagnostics for Windows - The Chromium Projects, accessed June 10, 2026, [https://www.chromium.org/Home/chromium-security/articles/chrome-sandbox-diagnostics-for-windows/](https://www.chromium.org/Home/chromium-security/articles/chrome-sandbox-diagnostics-for-windows/)  
47. What is Remote Browser Isolation (RBI) | Nomios Group, accessed June 10, 2026, [https://www.nomios.com/resources/remote-browser-isolation-rbi/](https://www.nomios.com/resources/remote-browser-isolation-rbi/)  
48. Prompt Injection - OWASP Foundation, accessed June 10, 2026, [https://owasp.org/www-community/attacks/PromptInjection](https://owasp.org/www-community/attacks/PromptInjection)  
49. How are you handling prompt injection in AI agents that read untrusted content? - Reddit, accessed June 10, 2026, [https://www.reddit.com/r/AskNetsec/comments/1rwywvu/how_are_you_handling_prompt_injection_in_ai/](https://www.reddit.com/r/AskNetsec/comments/1rwywvu/how_are_you_handling_prompt_injection_in_ai/)  
50. how-microsoft-defends-against-indirect-prompt-injection-attacks, accessed June 10, 2026, [https://www.microsoft.com/en-us/msrc/blog/2025/07/how-microsoft-defends-against-indirect-prompt-injection-attacks](https://www.microsoft.com/en-us/msrc/blog/2025/07/how-microsoft-defends-against-indirect-prompt-injection-attacks)  
51. Architectures for Agent Systems: A Survey of Isolation, Integration, and Governance | by yunwei37 | Medium, accessed June 10, 2026, [https://medium.com/@yunwei356/architectures-for-agent-systems-a-survey-of-isolation-integration-and-governance-59224d26e666](https://medium.com/@yunwei356/architectures-for-agent-systems-a-survey-of-isolation-integration-and-governance-59224d26e666)  
52. AI-ENG-Q — Speech, Voice, and Real-Time Interaction Systems

---

[← Back to Canon Map](../canon-map.md)