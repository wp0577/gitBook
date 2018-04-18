```


<!doctype html>
<html lang="en">

	<head>
		<!-- Required meta tags -->
		<meta charset="utf-8">
		<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

		<!-- Bootstrap CSS -->
		<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.0/css/bootstrap.min.css" integrity="sha384-9gVQ4dYFwwWSjIDZnLEWnxCjeSWFphJiwGPXr1jddIhOegiu1FwO5qRGvFXOdJZ4" crossorigin="anonymous">
		<!-- Optional JavaScript -->
		<!-- jQuery first, then Popper.js, then Bootstrap JS -->
		<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
		<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.0/umd/popper.min.js" integrity="sha384-cs/chFZiN24E4KMATLdqdvsezGxaGsi4hLGOzlXwp5UZB1LY//20VyM2taTB4QvJ" crossorigin="anonymous"></script>
		<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.0/js/bootstrap.min.js" integrity="sha384-uefMccjFJAIv6A+rW+L4AHf99KvxDjWSu1z9VI8SKNVmz4sk7buKt/6v9KI65qnm" crossorigin="anonymous"></script>

		<title>bootstrap web construction</title>
	</head>

	<body>
		<div class="container">
			<div class="row">
				<div class="col-lg-4 col-md-4">
					<img src="img/logo2.png" alt="">
				</div>
				<div class="col-lg-5 col-md-5">
					<img src="img/header.png" alt="">
				</div>
				<div class="col-lg-3 col-md-3" style="padding-top: 15px; weight: 100%">
					<a href="#">登陆</a>
					<a href="#">注册</a>
					<a href="#">购物车</a>
				</div>
			</div>
		</div>
		<div class="container" style="margin-top: 10px;">
			<nav class="navbar navbar-expand-lg navbar-dark bg-dark ">
				<a class="navbar-brand" href="#">Navbar</a>
				<button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
    					<span class="navbar-toggler-icon"></span>
  				</button>
				<div class="collapse navbar-collapse" id="navbarSupportedContent">
					<ul class="navbar-nav mr-auto">
						<li class="nav-item active">
							<a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
						</li>
						<li class="nav-item">
							<a class="nav-link" href="#">Link</a>
						</li>
						<li class="nav-item dropdown">
							<a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
								Dropdown
							</a>
							<div class="dropdown-menu" aria-labelledby="navbarDropdown">
								<a class="dropdown-item" href="#">Action</a>
								<a class="dropdown-item" href="#">Another action</a>
								<div class="dropdown-divider"></div>ß
								<a class="dropdown-item" href="#">Something else here</a>
							</div>
						</li>
						<li class="nav-item">
							<a class="nav-link disabled" href="#">Disabled</a>
						</li>
					</ul>
					<form class="form-inline my-2 my-lg-0">
						<input class="form-control mr-sm-2" type="search" placeholder="Search" aria-label="Search">
						<button class="btn btn-outline-success my-2 my-sm-0" type="submit">Search</button>
					</form>
				</div>
			</nav>
		</div>
		<div class="container" style="margin-top: 10px;">
			<div id="carouselExampleIndicators" class="carousel slide" data-ride="carousel">
				<ol class="carousel-indicators">
					<li data-target="#carouselExampleIndicators" data-slide-to="0" class="active"></li>
					<li data-target="#carouselExampleIndicators" data-slide-to="1"></li>
					<li data-target="#carouselExampleIndicators" data-slide-to="2"></li>
				</ol>
				<div class="carousel-inner">
					<div class="carousel-item active">
						<img class="d-block w-100" src="../WebJson/img/1.jpg" alt="First slide">
					</div>
					<div class="carousel-item">
						<img class="d-block w-100" src="../WebJson/img/2.jpg" alt="Second slide">
					</div>
					<div class="carousel-item">
						<img class="d-block w-100" src="../WebJson/img/3.jpg" alt="Third slide">
					</div>
				</div>
				<a class="carousel-control-prev" href="#carouselExampleIndicators" role="button" data-slide="prev">
					<span class="carousel-control-prev-icon" aria-hidden="true"></span>
					<span class="sr-only">Previous</span>
				</a>
				<a class="carousel-control-next" href="#carouselExampleIndicators" role="button" data-slide="next">
					<span class="carousel-control-next-icon" aria-hidden="true"></span>
					<span class="sr-only">Next</span>
				</a>
			</div>
		</div>
		<div class="container">
			<div class="row">
				<span style="margin-top: 10px; padding-left: 20px; font-size: 30px;">热门商品</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
				<img src="../WebJson/img/title2.jpg" style="height: 100%; margin-top: 30px;" />
			</div>

			<div class="row">
				<div class="col-lg-2 col-md-2">
					<img src="../WebJson/img/big01.jpg" />
				</div>
				<div class="col-lg-10 col-md-10" style="height: 100%;">
					<div class="row">
						<div class="col-lg-6">
							<img src="../WebJson/img/middle01.jpg" style="width: 100%; height: 100%; padding-left: 30px;" />
						</div>
						<div class="col-lg-2">
							<a href=""><img src="../WebJson/img/small03.jpg" /></a>
							<p align="center">
								<a href="#">电饭锅</a>
							</p>
							<p align="center">$399</p>

						</div>
						<div class="col-lg-2">
							<img src="../WebJson/img/small03.jpg" />
							<p align="center">
								<a href="#">电饭锅</a>
							</p>
							<p align="center">$399</p>
						</div>
						<div class="col-lg-2">
							<img src="../WebJson/img/small03.jpg" />
							<p align="center">
								<a href="#">电饭锅</a>
							</p>
							<p align="center">$399</p>
						</div>
					</div>
					<div class="row" style="padding-left: 30px; padding-top: 20px;">
						<div class="col-2">
							<img src="../WebJson/img/small03.jpg" />
							<p align="center">
								<a href="#">电饭锅</a>
							</p>
							<p align="center">$399</p>
						</div>
						<div class="col-2">
							<img src="../WebJson/img/small03.jpg" />
							<p align="center">
								<a href="#">电饭锅</a>
							</p>
							<p align="center">$399</p>
						</div>
						<div class="col-2">
							<img src="../WebJson/img/small03.jpg" />
							<p align="center">
								<a href="#">电饭锅</a>
							</p>
							<p align="center">$399</p>
						</div>
						<div class="col-2">
							<img src="../WebJson/img/small03.jpg" />
							<p align="center">
								<a href="#">电饭锅</a>
							</p>
							<p align="center">$399</p>
						</div>
						<div class="col-2">
							<img src="../WebJson/img/small03.jpg" />
							<p align="center">
								<a href="#">电饭锅</a>
							</p>
							<p align="center">$399</p>
						</div>
						<div class="col-2">
							<img src="../WebJson/img/small03.jpg" />
							<p align="center">
								<a href="#">电饭锅</a>
							</p>
							<p align="center">$399</p>
						</div>
					</div>
				</div>
			</div>
		</div>
		<div class="container" style="margin-top:10px;" >
			<img src="img/ad.jpg" style="width: 100%;"/>
		</div>
		<div class="container">
			<div class="row">
				<span style="margin-top: 10px; padding-left: 20px; font-size: 30px;">热门商品</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
				<img src="../WebJson/img/title2.jpg" style="height: 100%; margin-top: 30px;" />
			</div>

			<div class="row">
				<div class="col-lg-2 col-md-2">
					<img src="../WebJson/img/big01.jpg" />
				</div>
				<div class="col-lg-10 col-md-10" style="height: 100%;">
					<div class="row">
						<div class="col-lg-6">
							<img src="../WebJson/img/middle01.jpg" style="width: 100%; height: 100%; padding-left: 30px;" />
						</div>
						<div class="col-lg-2">
							<a href=""><img src="../WebJson/img/small03.jpg" /></a>
							<p align="center">
								<a href="#">电饭锅</a>
							</p>
							<p align="center">$399</p>

						</div>
						<div class="col-lg-2">
							<img src="../WebJson/img/small03.jpg" />
							<p align="center">
								<a href="#">电饭锅</a>
							</p>
							<p align="center">$399</p>
						</div>
						<div class="col-lg-2">
							<img src="../WebJson/img/small03.jpg" />
							<p align="center">
								<a href="#">电饭锅</a>
							</p>
							<p align="center">$399</p>
						</div>
					</div>
					<div class="row" style="padding-left: 30px; padding-top: 20px;">
						<div class="col-2">
							<img src="../WebJson/img/small03.jpg" />
							<p align="center">
								<a href="#">电饭锅</a>
							</p>
							<p align="center">$399</p>
						</div>
						<div class="col-2">
							<img src="../WebJson/img/small03.jpg" />
							<p align="center">
								<a href="#">电饭锅</a>
							</p>
							<p align="center">$399</p>
						</div>
						<div class="col-2">
							<img src="../WebJson/img/small03.jpg" />
							<p align="center">
								<a href="#">电饭锅</a>
							</p>
							<p align="center">$399</p>
						</div>
						<div class="col-2">
							<img src="../WebJson/img/small03.jpg" />
							<p align="center">
								<a href="#">电饭锅</a>
							</p>
							<p align="center">$399</p>
						</div>
						<div class="col-2">
							<img src="../WebJson/img/small03.jpg" />
							<p align="center">
								<a href="#">电饭锅</a>
							</p>
							<p align="center">$399</p>
						</div>
						<div class="col-2">
							<img src="../WebJson/img/small03.jpg" />
							<p align="center">
								<a href="#">电饭锅</a>
							</p>
							<p align="center">$399</p>
						</div>
					</div>
				</div>
			</div>
		</div>
	</body>

</html>
```



