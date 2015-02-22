function params = hw2trainbnb(X,Y)
    class_prior_number  = unique(Y)
    trainSize = length(Y);
    dim = size(X)
    dim = dim(2)
    class_prior = zeros(1,class_prior_number);
    for i = 1 : class_prior_number
        class_prior(i) = sum(Y==i)/trainSize;
    end
    
    MLE = zeros(class_prior_number,dim);
    for i = 1 : class_prior_number
        temp_array = data(labels==i,:);
        class_size = size(temp_array,1)
        jthEntry = sum(temp_array,1)';
        MLE(i,:) = (1+ jthEntry)/(2+class_size)
    end
    params.mle = MLE;
    params.prior = class_prior;
end

