<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WIICKED STUDIOS</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <!-- Firebase SDK -->
    <script src="https://www.gstatic.com/firebasejs/9.x.x/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.x.x/firebase-firestore.js"></script>
    <style>
        /* Previous styles remain... */

        /* Newsletter Styles */
        .newsletter-section {
            background: #f5f5f5;
            padding: 4rem 2rem;
            text-align: center;
        }

        .newsletter-content {
            max-width: 600px;
            margin: 0 auto;
        }

        .newsletter-form {
            display: flex;
            gap: 1rem;
            margin-top: 2rem;
        }

        .newsletter-form input {
            flex: 1;
            padding: 1rem;
            border: 1px solid var(--border-color);
            border-radius: 4px;
        }

        .newsletter-form button {
            padding: 1rem 2rem;
            background: var(--primary-color);
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }

        /* Size Guide Styles */
        .size-guide-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            z-index: 1100;
        }

        .size-guide-content {
            background: white;
            max-width: 800px;
            margin: 50px auto;
            padding: 2rem;
            border-radius: 8px;
            max-height: 90vh;
            overflow-y: auto;
        }

        .size-table {
            width: 100%;
            border-collapse: collapse;
            margin: 2rem 0;
        }

        .size-table th,
        .size-table td {
            border: 1px solid var(--border-color);
            padding: 1rem;
            text-align: center;
        }

        /* Care Instructions Styles */
        .care-instructions {
            margin-top: 2rem;
            padding: 2rem;
            background: #f5f5f5;
            border-radius: 8px;
        }

        .care-icons {
            display: flex;
            gap: 2rem;
            margin-top: 1rem;
            justify-content: center;
        }

        .care-icon {
            text-align: center;
        }

        .care-icon i {
            font-size: 2rem;
            margin-bottom: 0.5rem;
        }

        /* Success Message */
        .success-message {
            display: none;
            background: #4CAF50;
            color: white;
            padding: 1rem;
            border-radius: 4px;
            margin-top: 1rem;
        }
    </style>
</head>
<body>
    <!-- Previous content remains... -->

    <!-- Newsletter Section -->
    <section class="newsletter-section">
        <div class="newsletter-content">
            <h2>Join the WIICKED Community</h2>
            <p>Be the first to know about new drops, exclusive offers, and limited edition releases.</p>
            <form class="newsletter-form" id="newsletter-form">
                <input type="email" placeholder="Enter your email" required>
                <button type="submit">Sign Up</button>
            </form>
            <div class="success-message">
                Thanks for joining! Check your email to confirm your subscription.
            </div>
        </div>
    </section>

    <!-- Size Guide Modal -->
    <div class="size-guide-modal" id="size-guide-modal">
        <div class="size-guide-content">
            <h2>Size Guide</h2>
            <table class="size-table">
                <thead>
                    <tr>
                        <th>Size</th>
                        <th>Chest (cm)</th>
                        <th>Length (cm)</th>
                        <th>Shoulder (cm)</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>S</td>
                        <td>96-101</td>
                        <td>66-68</td>
                        <td>44-46</td>
                    </tr>
                    <tr>
                        <td>M</td>
                        <td>101-106</td>
                        <td>68-70</td>
                        <td>46-48</td>
                    </tr>
                    <tr>
                        <td>L</td>
                        <td>106-111</td>
                        <td>70-72</td>
                        <td>48-50</td>
                    </tr>
                </tbody>
            </table>
            <button class="close-modal">Close</button>
        </div>
    </div>

    <!-- Care Instructions -->
    <div class="care-instructions">
        <h3>Care Instructions</h3>
        <div class="care-icons">
            <div class="care-icon">
                <i class="fas fa-tint"></i>
                <p>Machine wash cold</p>
            </div>
            <div class="care-icon">
                <i class="fas fa-ban"></i>
                <p>Do not bleach</p>
            </div>
            <div class="care-icon">
                <i class="fas fa-temperature-low"></i>
                <p>Tumble dry low</p>
            </div>
            <div class="care-icon">
                <i class="fas fa-iron"></i>
                <p>Iron on low heat</p>
            </div>
        </div>
    </div>

    <script>
        // Firebase Configuration
        const firebaseConfig = {
            // Replace with your Firebase config
            apiKey: "your-api-key",
            authDomain: "your-auth-domain",
            projectId: "your-project-id",
            storageBucket: "your-bucket",
            messagingSenderId: "your-sender-id",
            appId: "your-app-id"
        };

        // Initialize Firebase
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();

        // Newsletter Signup
        const newsletterForm = document.getElementById('newsletter-form');
        const successMessage = document.querySelector('.success-message');

        newsletterForm.addEventListener('submit', async (e) => {
            e.preventDefault();
            const email = newsletterForm.querySelector('input').value;

            try {
                // Store in Firebase
                await db.collection('newsletter').add({
                    email: email,
                    signupDate: new Date()
                });

                // Show success message
                successMessage.style.display = 'block';
                newsletterForm.reset();

                // Hide success message after 3 seconds
                setTimeout(() => {
                    successMessage.style.display = 'none';
                }, 3000);
            } catch (error) {
                console.error('Error:', error);
                alert('Something went wrong. Please try again.');
            }
        });

        // Size Guide Modal
        const sizeGuideModal = document.getElementById('size-guide-modal');
        const closeModal = document.querySelector('.close-modal');

        document.querySelectorAll('.size-guide-link').forEach(link => {
            link.addEventListener('click', () => {
                sizeGuideModal.style.display = 'block';
            });
        });

        closeModal.addEventListener('click', () => {
            sizeGuideModal.style.display = 'none';
        });

        // Close modal on outside click
        window.addEventListener('click', (e) => {
            if (e.target === sizeGuideModal) {
                sizeGuideModal.style.display = 'none';
            }
        });

        // Previous JavaScript remains...
    </script>
</body>
</html>
