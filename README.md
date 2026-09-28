# EcoNovaa_detailed_report
#this contain some of the important and brief explanation about the whole project

1. PROBLEM BACKGROUND
Most waste in India is still not segregated at the source, even though the Solid Waste Management Rules, 2016 require it.
- Mixed waste contaminates recyclables such as plastic and metal, which lowers their value or makes them unusable.
- Sanitation workers have to sort unsegregated waste by hand and are exposed to hazardous material.
- Overflowing, unmonitored bins create unhygienic conditions at high-footfall places.
- People have little immediate reason to segregate, so compliance stays low even where separate bins exist.
- Existing smart bins mostly do only fill-level monitoring. EcoNova combines classification, sorting, monitoring and rewards in one system.
 
2. REWARD POINT SYSTEM
Tiers (points scale with recycling value):
- Organic waste: +[2] points
- Plastic waste: +[5] points
- Metal waste: +[8] points

Why tiered: a flat reward treats all waste equally. Tiering pushes users to separate high-value material correctly and reflects real recycling economics.
Reward flow:
1. Waste is classified and sorted.
2. A unique QR code is generated for that disposal.
3. The user scans the QR, logs in or signs up, and claims the points.
4. The dashboard shows total points and contribution history.

Anti-misuse controls:
- Each QR is single-use, time-limited, and tied to one bin and one event.
- Daily point cap per user.
- Login required to claim points.
- Points are credited only after sorting is confirmed.

Redemption: points can be redeemed for discounts or vouchers through partnered vendors, such as campus canteens or stores.

3. MIXED WASTE: PROBLEM, DRAWBACKS AND SOLUTION
Problem:
Real waste is often composite, such as a bottle with a metal cap, a foil-lined wrapper, or a paper cup with a plastic lid. One camera view can be classified differently from another.

Drawbacks if ignored:
- Misclassification: a single-label model has to pick one class.
- Contamination: one wrong item lowers the quality and resale value of the whole bin.
- Wrong rewards: a mixed item could earn high-tier points.
- Mechanical issues: irregular or oversized items can jam the servo diverter.

Our solution:
- Every prediction gets a confidence score. High-confidence items are auto-sorted.
- Low-confidence items are never force-sorted. The touchscreen asks the user to confirm the detected type or re-dispose the item, for example after removing the cap.
- The screen prompts users to separate caps, lids and residue before inserting.
- Points are given only for confirmed, sorted items.
- Low-confidence and corrected images can be logged and used to retrain the model.

Remaining limitation:
The fallback hands the decision to the user rather than fully solving composite waste. Sensor fusion (weight, moisture, metal detection) is the planned long-term fix.

4. EFFECT ON YOUNG USERS
Campuses, hostels and schools make students the main users, so the reward system works as a behaviour-change tool.

Positive effects:
- Immediate feedback: points appear right after disposal, which reinforces the action more than awareness campaigns do.
- Habit formation: small repeated rewards build a disposal routine.
- Social engagement: hostel-wise or department-wise leaderboards make disposal a shared, competitive activity.
- Long-term effect: habits formed on campus carry into homes and workplaces later.

Risks and mitigations:
- Novelty wears off: points are redeemable for real value, and streaks and leaderboards keep interest up.
- Gaming the system: daily caps and single-use QR codes limit abuse.
- Chasing points without understanding why: the app can carry short awareness content on why segregation matters. 
  
5.SANITIZATION ASPECT
The problem statement covers waste segregation, disposal and improved sanitization. EcoNova contributes in three ways:
- Touch-free disposal: the user inserts waste and the machine sorts it, so nobody handles mixed waste by hand.
- Fill-level monitoring: ultrasonic sensors detect when a bin is nearly full and send a collection alert, which prevents overflow.
- Safer work for sanitation staff: they handle pre-sorted, less contaminated waste.


6.TECHNICAL APPROACH
Hardware:
- Camera (USB/CSI): captures the waste image.
- IR sensor: detects insertion and triggers capture.
- Laptop / SBC [choose one]: runs YOLO11n, the touchscreen UI and QR generation.
- ESP32: receives the classification result, drives the servos, reads sensors and connects to Wi-Fi.
- Servo diverter: routes waste to the correct bin.
- Ultrasonic sensors: measure fill level in each bin.
- Touchscreen: shows guidance, results, confirmation prompts and the QR code.

Software:
- AI model: YOLO11n, with OpenCV for preprocessing.
- Front end: [React / HTML-CSS-JS] for the kiosk UI and dashboard.
- Back end: [Flask / FastAPI / Node] for the API, QR generation and point logic.
- Database: [Firebase / MongoDB] for users, points, disposal logs and bin levels.
- Deployment: [platform, or "planned for pilot phase"].

Data flow:
Insert waste -> IR trigger -> camera capture -> OpenCV preprocessing -> YOLO classification with confidence score -> [high confidence: auto-sort | low confidence: user confirms or re-disposes] -> ESP32 command -> servo sorts -> log to cloud -> QR generated -> user claims points -> dashboard updates.

Training data:
[TrashNet / TACO / custom images]. TrashNet has six classes (plastic, metal, paper, cardboard, glass, trash), so state how you map them to Organic / Plastic / Metal.

Offline behaviour:
Sorting runs locally and logs sync when Wi-Fi returns. QR claiming needs a connection. 


7.DATA PRIVACY AND SECURITY
- Collect only what rewards need: name, mobile or email, and points history.
- Store passwords hashed, never in plain text.
- Use standard authentication Firebase Auth 
- Use camera images only for classification. Keep an image only if a low-confidence one is saved for retraining. 
- Use HTTPS between the kiosk, back end and app.
  
8.IMPLEMENTATION ROADMAP
Phase 1: Train and validate the classification model.
Phase 2: Build the physical sorting prototype (camera, IR sensor, ESP32, servo, bins).
Phase 3: Build the QR, login and reward system.
Phase 4: Build the dashboard and fill-level monitoring.
Phase 5: Pilot at one campus location and gather usage data.
Phase 6: Expand to more bins and institutions.

9. RISKS AND MITIGATIONS
- Mixed or dirty waste -> confidence threshold and user confirmation
- Uncertain AI predictions -> low-confidence path and retraining on logged images
- Device failure -> modular parts, backup hardware, scheduled maintenance
- False fill-level readings -> sensor calibration and repeated readings
- Low usage -> rewards, leaderboards, redeemable points
- Poor lighting -> fixed camera position with internal LED lighting
- Vandalism or misuse -> sturdy enclosure, single-use QR, daily caps
- Wi-Fi loss -> local sorting continues, data syncs later

Current limitations:
- The system is at the design stage; a fully integrated physical
  prototype is not yet tested.
- Accuracy targets are projections until measured.
- Composite waste is only partly solved by the fallback.
- Long-term effect of rewards on behaviour is untested with real
  users.
- Moving parts need periodic maintenance.

10. FUTURE SCOPE
- Add hazardous waste, e-waste and battery categories.
- Add weight, moisture and metal sensors alongside the camera.
- Improve the model with real usage data.
- Integrate with smart-city and municipal systems.
- Build a dedicated mobile app for tracking, rewards and awareness.
- Partner with recyclers and composters.
- Deploy at scale across campuses, malls, metro stations and societies.
