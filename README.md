h1. Expedia

Expedia is a ruby wrapper for "EAN (Expedia Affiliate Network)":www.expediaaffiliate.com APIs.

Other details of this gem are:

* It use the the latest verion of the EAN API. i.e "Version 3":http://developer.ean.com/docs/read/hotels/version_3
* Only REST API support (No XML or SOAP support)

h2. Installation

For Rails Add this line to your application's Gemfile:

<pre>
gem 'expedia'
</pre>

And then execute:

<pre>
$ bundle
</pre>

Or install it yourself as:

<pre>
$ gem install expedia
</pre>

Then execute the following command to create an intializer:

<pre>
$ rake expedia:initialize
</pre>

h2. Usage

After configuring keys for your EAN application use

<pre>
# Instentiate api object
api = Expedia::API.new

# Method to search hotel. see http://developer.ean.com/docs/read/hotels/version_3/request_hotel_list
response = api.get_list({:propertyName => 'Hotel Moa Berlin', :destinationString => 'berlin'})
</pre>

Following methods are expeosed by Expedia::API object

Note: All method naming is done in correspondence with Expedia services and ruby conventions
see "Hotel API Documentation (Services section)":http://developer.ean.com/docs/read/hotels#.UMf_hiNDt0w
<pre>
get_list({})
geo_search({})
get_availability({})
get_room_images({})
get_information({})
get_rules({})
get_itinerary({})
get_alternate_properties({})
get_reservation({})
get_payment_info({})
get_cancel({})
get_ping({})
get_static_reservation({}) # To test Reservation (Static Reservation)
</pre>

Every method accepts Hash of parameter specific to every API. see "EAN Docs":http://developer.ean.com/docs/read/hotels/version_3 for more details. 

h3. Success

if request is successfull you will get a Expedia::HTTPService::Response object in response.
and you can use

<pre>
response.status
response.body
response.headers
</pre>

h3. Error

In case of any error a Expedia::APIError object is returned.

Note: Expedia respondes with status of 200 even if there is an exception (most of the times). So no Exception is raised!

<pre>
# See http://developer.ean.com/docs/read/error_handling/Hotel_V3_Exception_Details

response.error_body # Complete error body
response.error_body # Response status
response.category # Value indicating the nature of the exception or the reason it occurred
response.presentation_message # Presentation error message returned
response.verbose_message # More specific detailed error message
response.handling # value indicating the severity of the exception and how it may be handled
</pre>

h2. Test Booking (Static Reservation)

For Static reservation use get_static_reservation() method.

CAUTION: Do Not send adress and booking information (creditCardNumber, creditCardIdentifier, creditCardExpirationMonth, creditCardExpirationYear, address1) in parameters to the method. Especially do not pass address1 parameter They are already been taken care of. For more on Static booking see "Static Test Booking Credit Card Information":http://developer.ean.com/docs/Test_Booking_Procedures

A static Booking example.

<pre>
response = api.get_static_reservation({	:arrivalDate => "10/10/2013", :departureDate => "10/12/2013", :hotelID => 359433, 
  :supplierType => "E", :rateKey => "084eab14-335e-46d6-aa2e-766fce6be32c", :roomTypeCode => 200007964,
  :rateCode => 200865704, :chargeableRate => "142.8",
  :room1 => "1", :room1FirstName => "test", :room1LastName => "testers", :room1BedTypeId => "15",
  :room1SmokingPreference => "NS", :email => "test@tesing.com", :city => 'Bellevue', :stateProvinceCode => 'WA',
	:countryCode => 'US', :postalCode => 98004 })
</pre>
