<script>
    import jwt_decode from "jwt-decode";

    var login = {
        email: "",
        password: "",
    };

    let error = "";

    async function handleSubmit(e) {
        e.preventDefault();
        console.log(login);
        api_post_login();
    }

    async function api_post_login() {
        const response = await fetch(`${process.env.API_URL}/auth/login`, {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify(login),
        });

        if (response.status == 401) {
            error = "Username or password is incorrect.";
        }

        const data = await response.json();

        if (data) {
            localStorage.setItem("token", data.auth_token);
            var user = await jwt_decode(data.auth_token);
            localStorage.setItem("userId", JSON.stringify(user.user_id));
            navigateHome();
        }
    }

    function navigateHome() {
        window.location.href = "/";
    }
</script>

<style>
    main {
        display: flex;
        flex-direction: column !important;
        justify-content: center;
        align-items: center;
        height: 100%;  
    }

    a {
        color: rgb(241, 174, 86);
    }
    a:hover {
        text-decoration: underline;
        cursor: pointer;
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
        min-width: 20vw;
        background-color: rgba(31, 31, 31, 0.89);
        /* border: 1px solid rgb(155, 37, 102); */
        flex-direction: column;
        border-radius: 0.35em;
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
        outline-color: rgb(241, 174, 86);
    }

    button {
        margin-top: 25px;
        border-width: 2px;
        text-shadow: none !important;
    }

    button:hover {
        cursor: pointer;
        border: 2px solid rgb(241, 174, 86);
        border-radius: 0.25em;
    }
</style>

<main>
    <form on:submit={handleSubmit} class="login-form">
        <h1>Login</h1>

        {#if error != ''}
            <div class="error">{error}</div>
        {/if}

        <!-- E-Mail -->
        <label for="Email">Email Address</label>
        <input
            bind:value={login.email}
            required
            type="email"
            placeholder="example@email.com" />

        <!-- Password -->
        <label for="fullname">Password</label>
        <input bind:value={login.password} required type="password" />

        <p>
            Don't have an account?
            <a href="/signup">Click here to sign up.</a>
        </p>

        <button type="submit"> Confirm </button>
    </form>
</main>
