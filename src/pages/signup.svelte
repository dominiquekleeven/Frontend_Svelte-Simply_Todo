<script>
    import jwt_decode from "jwt-decode";

    var signup = {
        name: "",
        email: "",
        password: "",
        password_confirmation: "",
    };

    let error = "";

    async function handleSubmit(e) {
        e.preventDefault();
        console.log(signup);

        if (signup.password != signup.password_confirmation) {
            error = "Passwords do not match, please try again.";
        } else {
            api_PostSignUpForm();
        }
    }

    async function api_PostSignUpForm() {
        const response = await fetch(`${process.env.API_URL}/signup`, {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify(signup),
        }).catch((exc) => {
            error = exc;
        });
        const data = await response.json();

        if (data) {
            localStorage.setItem("token", data.result.auth_token);
            var user = jwt_decode(data.auth_token);
            localStorage.setItem("userId", JSON.stringify(user.user_id));
            navigateHome();
        }
    }

    function navigateHome() {
        window.location.href = "/";
    }
</script>

<style>
    a {
        color: rgb(155, 37, 102);
    }
    a:hover {
        text-decoration: underline;
        cursor: pointer;
    }

    .signup {
        
        display: flex;
        flex-direction: column !important;
        justify-content: center;
        align-items: center;
        height: 100%;  
    }

    .error {
        margin-top: 10px;
        margin-bottom: 10px;
        font-size: 18px;
        color: rgb(179, 49, 49);
    }

    @media (max-width: 700px) {
        .login-form {
            max-width: 80vw !important;
        }
    }

    .login-form {
        display: inline-flex;
        min-width: 25vw;
        min-height: 25vh;
        /* border: 1px solid rgb(155, 37, 102); */
        flex-direction: column;
        border-radius: 0.25em;
        padding: 25px;
        box-shadow: 0px 3px 4px 2px rgba(0, 0, 0, 0.308);
    }

    h1 {
        margin: 0;
        padding: 0;
        font-weight: 300;
    }

    label {
        margin-top: 15px;
    }

    input {
        margin-top: 5px;
        border-radius: 0.25em;
    }

    input:focus {
        outline-color: rgb(155, 37, 102);
    }

    button {
        margin-top: 25px;
        border-width: 2px;
    }

    button:hover {
        cursor: pointer;
        border: 2px solid rgb(155, 37, 102);
        border-radius: 0.25em;
    }
</style>

<main class="signup">
    <form on:submit={handleSubmit} class="login-form">
        <h1>Sign up</h1>

        {#if error != ''}
            <div class="error">{error}</div>
        {/if}

        <!-- Name -->
        <label for="Fullname">Full Name</label>
        <input
            bind:value={signup.name}
            required
            type="text"
            placeholder="John Doe" />

        <!-- E-Mail -->
        <label for="Email">Email Address</label>
        <input
            bind:value={signup.email}
            required
            type="email"
            placeholder="example@email.com" />

        <!-- Password -->
        <label for="fullname">Password</label>
        <input bind:value={signup.password} required type="password" />

        <!-- Confirm password -->
        <label for="fullname">Confirm Password</label>
        <input
            bind:value={signup.password_confirmation}
            required
            type="password" />

        <p>
            Already have an account?
            <a href="/login">Click here to login.</a>
        </p>

        <button type="submit"> Confirm </button>
    </form>
</main>
