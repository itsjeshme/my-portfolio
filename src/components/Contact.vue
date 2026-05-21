<script setup>
	import { ref, onMounted, onBeforeMount } from 'vue';

	import { Notyf } from 'notyf';
	import 'notyf/notyf.min.css';

	const notyf = new Notyf();

	const WEB3FORMS_ACCESS_KEY = "191747e6-4adc-4d6c-bd52-24c01d6f158f";

	const subject = "New message from Portfolio Contact Form";

	const name = ref("");
	const email = ref("");
	const message = ref("");

	const isLoading = ref(false);


	const submitForm = async() => {

		if(!recaptchaToken.value) {
			notyf.error('Please verify that you are not a robot.');
			return;
		}

		isLoading.value = true;

		try {

			const response = await fetch("https://api.web3forms.com/submit", {
				method: "POST",
				headers: {
					"Content-Type": "application/json",
					Accept: "application/json"
				},
				body: JSON.stringify({
					access_key: WEB3FORMS_ACCESS_KEY,
					subject: subject,
					name: name.value,
					email: email.value,
					message: message.value
				})
			});

			const result = await response.json();

			if(result.success) {
				console.log(result)

				isLoading.value = false;
				notyf.success("Message sent!");
			}

		} catch(error) {
			console.log(error);

			isLoading.value = false;
			notyf.error("Failed to send message");

		} finally {

			resetRecaptcha();
		}
	}

	const SITE_KEY = '6LdyM_UsAAAAAG5HdeGp7H_G129ytnSuuvDqDdYr';

	const recaptchaContainer = ref(null);
	const recaptchaWidgetId = ref(null);
	const recaptchaToken = ref('');


	function onRecaptchaSuccess(token) {
		recaptchaToken.value = token;
	}

	function onRecaptchaExpired() {
		recaptchaToken.value = '';
	}

	function renderRecaptcha() {
		if(!window.grecaptcha) {
			console.error('reCAPTCHA not loaded');
			return;
		}

		recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
			sitekey: SITE_KEY,
			size: 'normal',
			callback: onRecaptchaSuccess,
			'expired-callback': onRecaptchaExpired
		});
	}

	function resetRecaptcha() {
		if(recaptchaWidgetId.value !== null) {
			window.grecaptcha.reset(recaptchaWidgetId.value);
			recaptchaToken.value = '';
		}
	}

	onMounted(() => {
		const interval = setInterval(() => {
			if(window.grecaptcha && window.grecaptcha.render) {
				renderRecaptcha();
				clearInterval(interval)
			}
		}, 100);

		onBeforeMount(() => {
			clearInterval(interval);
		});
	})

</script>

<template>
	<section id="contact" class="container pb-5">
			<h1>Let's work together!</h1>
			<div class="row gap-4">
				<div class="col-12 col-md-6 pt-3">
					<div id="form" aria-label="Contact Form">
						<form @submit.prevent="submitForm"  class="p-3">
							<div>
								<label class="d-block" for="full_name">Full Name:</label>
								<input type="text" v-model="name" name="full_name" id="full_name" required autocomplete placeholder="First Name, MI, Last Name" class="form-control">
							</div>

							<div>
								<label class="d-block" for="email_add">Email Address:</label>
								<input type="email" v-model="email" name="email_add" id="email_add" required autocomplete placeholder="message@email.com" class="form-control">
							</div>

							<div>
								<label class="d-block" for="dm">Message:</label>
								<textarea name="dm" v-model="message" rows="3" id="dm" class="form-control"></textarea>
							</div>

							<div>
								<button type="submit" class="submit-btn pl-5 pr-5" :disabled="isLoading">{{isLoading ? "Sending..." : "Submit"}}</button>
							</div>

							<div class="d-flex justify-content-end mt-2">
								<div ref="recaptchaContainer"></div>
							</div>
						</form>
					</div>
					<div class="socials pt-5" aria-label="Social Media Accounts">
						<h4>Socials:</h4>
						<a href="https://github.com/itsjeshme" target="_blank"><img src="/images/githublogo.png" alt="github logo"></a>
						<a href="https://www.linkedin.com/in/jesiah-catindig/" target="_blank"><img src="/images/linkedinlogo.png" alt="linkedin logo"></a>
					</div>
				</div>
				<div class="col-12 col-md-5 py-3">
					<div id="gmap" aria-label="Map of Laguna">
						<iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d989839.9735591554!2d120.6604908690493!3d14.278723969582318!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x33bd600ea02bf27d%3A0xd19ea6f21743ff24!2sLaguna!5e0!3m2!1sen!2sph!4v1776606174894!5m2!1sen!2sph" width="600" height="450" style="border:0;" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>
					</div>
				</div>
			</div>
		</section>
</template>